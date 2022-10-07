<!-- START_METADATA

title: Settlement Guide
sidebar_position: 3

---

END_METADATA -->

# Vipps Settlement Guide

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

<!-- START_TOC -->

# Table of contents

* [Transaction types](#transaction-types)
  * [Capture transactions](#capture-transactions)
  * [Refund transactions](#refund-transactions)
  * [Payout transactions](#payout-transactions)
  * [Payout process](#payout-process)
  * [Other transactions](#other-transactions)
* [Questions?](#questions)

<!-- END_TOC -->

Document version: 0.0.4.


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
through
[portal.vipps.no](https://portal.vipps.no).
Refunds are always deducted
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

### Payout process

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

A day starts and ends at midnight, Oslo time: Start `00:00:00`, end `23:59:59`
(subseconds not specified).

The payout will be marked with the text `Utbet. 2000101 Vippsnr <ledgername>`.

**Please note:** We plan to later add a `GET:/report/v1/report/v1/payouts`
endpoint to the API that provides more information about the status of payouts.

### Other transactions

Users of this API has to account for the possibility of
more transaction types than just capture and refunds. Likely examples in the
future are invoices, chargebacks, manual adjustments, and weekly or
monthly fees. The rAPI specification has more details about
transaction types.

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
