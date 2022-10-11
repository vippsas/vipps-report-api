<!-- START_METADATA
---
title: "API Guide: Settlements"
sidebar_position: 7
---
END_METADATA -->

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

# Vipps Report API: Settlements

<!-- START_TOC -->

## Table of contents

* [Overview](#overview)
* [Ledgers](#ledgers)
* [Transaction types](#transaction-types)
  * [Capture transactions](#capture-transactions)
  * [Refund transactions](#refund-transactions)
  * [Payout transactions](#payout-transactions)
  * [Other transactions](#other-transactions)
* [Reports](#reports)
  * [Periodization](#periodization)
* [Questions?](#questions)

<!-- END_TOC -->

Document version: 0.0.6.

## Overview

Merchants using Vipps will receive the money in bulk payments, usually one per
day. In these bulk payment we have summed together all payments for the day,
subtracting refunds and fees to Vipps. It is therefore necessary to download
reports that explain the bulk payment, so that it can be correctly filed in the
merchant's accounting system. Such reports can be fetched either
in [the portal](https://portal.vipps.no) or by using the Report API.

Usually, you will wish to implement a *reconciliation process*, where
you download a report from Vipps each day, and check that contents
of the report match the data you have on your own side.
We recommended that you do this by matching per transaction on transaction IDs.

This guide will focus on using the Report API, but may also be useful reading
for those who rely on using reports from the portal for their reconciliation
processes.

## Ledgers

Vipps settlements work in the same way for all Vipps payment products; whether
one is using Vippsnummer, the eCom/ePayment API, the Checkout API or the
Recurring API.

Vipps does not transfer money to/from the merchant for every payment made.
Instead, all transactions are put on a *ledger*
that track the funds that Vipps owes the merchant. During the day transactions
occur that usually increase, and sometimes decrease, the balance the merchant
has in Vipps and thus the *ledger balance*. Periodically, usually daily, the
balance of the ledger is paid out to a configured account number and the balance
is reset.

The following illustration shows an example day at a low traffic merchant.
*Captures* and *refunds* (described in further detail below) are added to the
ledger, changing the balance of funds that Vipps owes the merchant. In the end,
the balance is paid out. The payout is itself a transaction on the ledger,
adjusting the balance down to zero.
![ledger balance illustration](./images/ledger-balance-simple.png)

For the large majority of merchants, there is a direct correspondence between a
Vippsnummer or e-com Merchant Serial Number (MSNs) to a ledger:

![ledger vs units, one to one](./images/ledger-vs-units-one-to-one.png)

However, for merchants who require it, Vipps have
limited support for multiple Vippsnummers/e-com MSNs to be settled together.
The payments to multiple different units are then combined in a
single settlement payout:

![ledgers vs units, many to one](./images/ledger-vs-units-one-to-many.png)

The ledger has its own `ledgerId`, so the first step in using the report API is
to fetch the list of ledgers you have access to. If you are integrating a single
merchant it may be enough to hit this endpoint once manually to identify
the `ledgerId`. An example response from
[`GET:/report/v1/ledgers`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1v1~1ledgers/get)
is:

```json
{
  "items": [
    {
      "ledgerId": "302321",
      "currency": "NOK",
      "payoutBankAccount": {
        "country": "NO",
        "accountNumber": "86011117947"
      },
      "firstPayout": "2000001",
      "lastPayout": "2000045",
      "owner": {
        "jurisdiction": "NO",
        "id": "987654321"
      },
      "settlesFor": [
        {
          "type": "epayment",
          "msn": "123455"
        }
      ]
    }
  ]
}
```

A Vippsnummer will have a different `settlesFor` structure:

```json
{
  "settlesFor": [
    {
      "type": "vippsnummer",
      "country": "NO",
      "vippsnummer": "123455"
    }
  ]
}
```

If you only want to look up the `ledgerId` from an MSN or Vippsnummer, you
may use the `msn` or `vippsnummer` arguments to filter the response.

If you are integrating an accounting system for many customers, it can be
relevant to poll this endpoint many times as you will continue to see new
ledgers appear for different customers as they
[grant your accounting system access to their data](vipps-report-api.md#give-access-to-an-accounting-partner).

## Transaction types

### Capture transactions

Captures represent money being transferred from the customer,
through Vipps, to the merchant. Captures increase the ledger balance. The
captures are indicated by a gross amount and a fee to Vipps for the service
provided; the balance is typically increased by the gross amount less the fee
(*net settlement*).

In exceptional cases Vipps can configure *gross settlements*.
In this case the fees are not added to the ledger balance,
but instead invoiced at the end of the month.

In some of the other APIs we distinguish between two
types of payment flows: The "sale" flow for an immediate purchase,
and "reserve/capture" flow for where one first receives a reservation,
and then capture it fully or partially at a later point. In the settlement
process we use *capture* for both cases for a transfer from the customer
to the merchant through Vipps. Reservations are not relevant to
the settlement process.

### Refund transactions
Refunds represent transfers in the other direction. These are
initiated by the merchant; either by using the API or
through [portal.vipps.no](https://portal.vipps.no). Refunds are always deducted
from the next settlement payout, also if you have a gross settlement setup.
Currently, refunds always have zero fees.

### Payout transactions

Payouts represent a transfer from the merchant's ledger, sitting
in Vipps' bank accounts, to the bank account of the merchant. Since the money
goes *out of* the ledger, and *to* the merchant's bank account, the payout
transaction has a negative amount in the figure above.

With the typical daily settlement setup, there will be a payout transaction
generated every midnight for the full balance on the ledger. There are however
exceptions to this:

* Sometimes the ledger balance becomes negative, because the sum of refunds
  exceeds the sum of captures minus the sum of fees. In these cases there is
  nothing to pay out and the payout transaction will not be generated.
  The merchant is loaning money from Vipps in order to refund the customer. Once
  the ledger balance is positive again the loan has been repaid and payouts
  resume. An example is shown in the figure further down on this page.

* It is possible to configure a Ledger to generate payouts on a weekly or
  monthly basis. This is mainly done by merchants with low volume who want to
  save the accounting overhead of booking a payout every day.

* Depending on the specific merchant, Vipps may require that some money is left
  on the ledger in case of future refunds or chargebacks. It should therefore
  not be assumed that the balance on the ledger is zero after a payout.

* Vipps may in general block payouts from a given ledger due to
  special agreements with the merchant in order to mitigate risk,
  suspicion of fraud, etc.

The presence of a payout transaction on the ledger simply indicates
a *decision to pay the money out*.
There is a two-day delay in the bank transfer process itself:

* Day 1: A *sale* or *capture* happens. Since a merchant should not capture the
  amount, i.e. charge the customer, until the purchased product is shipped,
  the "day 1" is normally the day that the product is shipped and the customer's
  account is charged.

* Day 2: The *payout transaction* is generated during the night, and the
  payout transaction becomes visible on the ledger. The timestamp
  is set to midnight at the start of the day.

* Day 3: The actual bank transfer for the payout is made from Vipps' bank
  account to the merchant's bank account. Money is normally available in the
  account before noon.

Days are bank days, Monday - Friday, excluding banking holidays. In other words,
a capture made on Monday will be on merchant's account on Wednesday, while a
capture made on Friday will be on merchant's account on Tuesday.

The payout will be marked with the text `Utbet. 2000101 Vippsnr <ledgername>`.

In the future, we plan to add a
`GET:/report/v1/payouts`
endpoint to the API that provides more information about the status of a payout.

### Other transactions

Users of this API has to account for the possibility of
more transaction types than just capture and refunds. Likely examples in the
future are invoices, chargebacks, manual adjustments, and weekly or
monthly fees. The reference documentation has more details about
transaction types.

## Reports

To perform reconciliation you download a *report* that lists the transactions
that has happened on a specific ledger. To continue with our simple example from
above:
![ledger balance illustration](./images/ledger-balance-simple.png)

One can request a report from this ledger by
calling
[`GET:/report/v1/ledgers/{ledgerId}/transactions`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1v1~1ledgers~1%7BledgerId%7D~1transactions/get),
for instance:

```
GET https://api.vipps.no/report/v1/ledgers/302321/transactions?ledgerDate=2022-10-01&columns=transactionId,transactionType,reference,ledgerDate,ledgerAmount,grossAmount,fee,msn,time,price.description
```

The endpoint can return either CSV or JSON depending on the `Accept` header;
both always contain the exactly same data just in different representations. An
example CSV response for the call above that matches the illustration above:

```
transactionId,transactionType,reference,ledgerDate,ledgerAmount,grossAmount,fee,msn,time,price.description
3343121302,capture,purchase-12,2022-10-01,97.00,100.00,3.00,123455,2022-10-01T10:23:43.422143+02:00,3.00% + 0.00
2342128799,capture,purchase-12,2022-10-01,97.00,100.00,3.00,123455,2022-10-01T11:04:12.234234+02:00,3.00% + 0.00
2442145459,capture,purchase-13,2022-10-01,194.00,200.00,6.00,123455,2022-10-01T13:11:40.234234+02:00,3.00% + 0.00
2049872323,refund,purchase-12,2022-10-01,-100.00,-100.00,0.00,123455,2022-10-01T14:32:17.324342+02:00,
18000302321002000045,payout,2000045,2022-10-01,-288.00,-288.00,0.00,,2022-10-02T00:00:00.000000+02:00,
```

Formatted as a table:

| transactionId        | transactionType | reference   | ledgerDate  | ledgerAmount | grossAmount |  fee | msn    | time                              | price.description |
|----------------------|-----------------|-------------|-------------|-------------:|------------:|-----:|--------|-----------------------------------|--------------|
| 3343121302           | capture         | purchase-12 | 2022-10-01  |        97.00 |      100.00 | 3.00 | 123455 | 2022-10-01T10:23:43.422143+02:00 | 3.00% + 0.00 |
| 2342128799           | capture         | purchase-12 | 2022-10-01  |        97.00 |      100.00 | 3.00 | 123455 | 2022-10-01T11:04:12.234234+02:00 | 3.00% + 0.00 |
| 2442145459           | capture         | purchase-13 | 2022-10-01  |       194.00 |      200.00 | 3.00 | 123455 | 2022-10-01T13:11:40.234234+02:00 | 3.00% + 0.00 |
| 2049872323           | refund          | purchase-12 | 2022-10-01  |      -100.00 |     -100.00 | 3.00 | 123455 | 2022-10-01T14:32:17.324342+02:00 |              |
| 18000302321002000045 | payout          | 2000045     | 2022-10-01  |      -288.00 |     -288.00 | 0.00 |        | 2022-10-02T00:00:00.000000+02:00 |              |

Some notes:

* You should be prepared to receive a new *transactionType* that you do not
  already know about.
* *ledgerAmount* is always the contribution the transaction has to the ledger
  balance. This means that if you are set up with gross settlements, it will be
  equal to `grossAmount`, unlike the case above.
* *ledgerDate* is the accounting date used to group transactions for payouts. In
  the future it may be possible to set this to something else than midnight
  local time, and in that case this will deviate from `time`.
* The payout transaction does not have an `msn`. The `msn` is not a required
  field, it represents metadata about a transaction. For Vippsnummer, `msn`
  is blank and instead the `vippsnummer` column can be requested.

For more details about individual columns available, please consult the
Swagger [TODO].

**Please note**: Data is not available in the API until some time after
the `ledgerDate` has ended. This is primarily because Vipps in some
cases compute fees based on the volume throughout the entire day,
so that the `fee` and `ledgerAmount` can not be computed before the day
has ended.

### Periodization

The
[`GET:/report/v1/ledgers/{ledgerId}/transactions`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1v1~1ledgers~1%7BledgerId%7D~1transactions/get)
endpoint has several parameters for selecting a range of
transactions to return, which can be used for an initial data import.

Most users of the API will want to set up an automated job to call
the
[`GET:/report/v1/ledgers/{ledgerId}/transactions`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1v1~1ledgers~1%7BledgerId%7D~1transactions/get)
endpoint on a daily basis to download the data for the
preceding day. Such synchronization can be done in two ways: Date-based indexing
and payout-based indexing. Often they will give the same results; the difference
is:

* When weekly or daily settlements are configured; then a payout is only
  generated the 1st day of the week or month
* When the balance is negative, a payout is not generated

The last scenario is depicted in the figure below. On the
time `2022-09-03 00:00:00`, the balance is negative, and so no payout is
generated for `ledgerDate=2022-09-02`.

* By fetching transactions by `?ledgerDate=`, one will get one report per date.
  This is indicated by the blue bands on top of the figure. The advantage is
  that one consistently gets more data each day, also for monthly/weekly
  settlements and negative balance.

* By fetching transactions by `?inPayout=`, transactions for several days
  may be returned in the same report. See the red bands at the bottom
  of the figure below. The advantage is that the sum of `ledgerAmount` in the
  report will exactly match the payout bank transfer amount.

Both modes can be useful depending on the specifics of your reconciliation
routines, but in general we recommend fetching by date, and to do reconciliation
transaction by transaction based on `reference`.

![Settlement](./images/report-periods.png)

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-report-api/issues),
a [pull request](https://github.com/vippsas/vipps-report-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
