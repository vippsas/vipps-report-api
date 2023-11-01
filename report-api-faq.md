---
title: Report API Frequently Asked Questions
sidebar_label: FAQ
sidebar_position: 60
description: Frequently asked questions for the Report API.
pagination_next: null
pagination_prev: null
---

# Frequently asked questions

Here are the Report API FAQs.
See the
[Report API guide](./api-guide/README.md)
for all the details.

For general information and questions, please check in the
[Knowledge base](https://developer.vippsmobilepay.com/docs/knowledge-base/).


## What are the benefits of the Report API over the SFTP service?

The
[SFTP Report (deprecated)](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service)
lets merchants and partners download settlement files by SFTP.

The Report API has several benefits over the SFTP sercvice:

* Accounting partners have one set of API keys for all their merchants:
  [Accounting keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys/#types-of-partner-keys).
  The accounting keys can not be used to make payments.
* Merchant can easily
  [give access to an accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner)
  on
  [portal.vipps.no](https://portal.vipps.no)
  with no coding by the merchant or the accounting partner.
  * The SFTP service required each merchant to enter the accounting partner's SSH key.
    Some merchants even shared their API keys, which enable the accounting partner to
    make payments, which is not allowed.
* Data can be retrieved at any time, for any time period.
  * The SFTP service only offered daily data: One file per day.
* Data is available regardless of whether the balance is positive or negative,
  so even if there have only been refunds, there will be data available.
  * The SFTP service only provided settlement data when the balance was positive.
* It is possible to retrieve data for more than one day in a request,
  for example for an entire weekend, week, or month.
  * The SFTP service only provided one file per day.
* The data is in JSON format, making it easy to use in different ways.
  * The SFTP service offered XML, XSLX, CSV and PDF, but most users now want JSON.

## Why does a merchant need to log in on portal.vipps.no to select accounting partner?

An accounting partner gets access to the merchant's sales unit's data.
We require the merchant to log in securely on
[portal.vipps.no](https://portal.vipps.no)
to provide this access and consent to sharing the sata with the accounting partner.

## Can a partner specify the accounting partner for a sales unit?

No, please see
[Why does a merchant need to log in on portal.vipps.no to select accounting partner?](#why-does-a-merchant-need-to-log-in-on-portalvippsno-to-select-accounting-partner).

## Can one sales unit have multiple accounting partners?

Yes. One sales unit may give access to several accounting partners,
and the "accounting partner" may be a CRM systems vendor or something other than
a pure _accounting_ vendor.

As long as the external party has applied to become a partner, and is visible in
the list of accounting partners, the merchant may give it access to one or more sales
units.

## What information can I get hold of?

Right now the only data that is available is the same data that is already
available in the settlement reports on
[portal.vipps.no](https://portal.vipps.no),
or on
[SFTP report (deprecated)](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

The only difference is that the data can be fetched over a more modern REST API.
We aim to provide more information through this API in the future.

**Important:** It is not yet possible to use the Report API to retrieve data
for *Vippsnummer* sale units. The Report API can only be used to sale units
that have API access (which *Vippsnummer* sale units do not have.)

## Can I get realtime data?

No. The Report API _may_ be extended to contain more information later,
and this FAQ will be updated if there are any changes.
There are no specific plans to do this yet.

## Why are the users' names, transaction texts, etc. not available in the Report API?

The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.

The reports on
[portal.vipps.no](https://portal.vipps.no)
provide more details.
The users' names, transaction texts and other information may be regulated by GDPR
and therefore require consent from the merchant. This consent may be given on
[portal.vipps.no](https://portal.vipps.no),
but not when using the Report API.

The Report API _may_ be extended to contain more information later,
and this FAQ will be updated if there are any changes.
There are no specific plans to do this yet.

## Is the Report API available for the test environment?

No. The
[test environment](https://developer.vippsmobilepay.com/docs/test-environment)
does not have the backend systems necessary to offer the Report API.

The only way to test the Report API is in the production environment.

Since the Report API is read-only, and the overall logic is very simple
(first you ask for data, then you receive that data), developing and testing in
the production environment is straight-forward.

Implementing the Report API in the test environment is a big effort,
as there are no settlements being made in that environment. The data that
the Report API needs is simply not available.
It is unlikely that we'll be able to prioritize this over other development efforts.

## Can a merchant find the Ledger ID for an MSN on portal.vipps.no?

No. For now, the only way to get the Ledger ID is to call
[`GET:settlement/v1/ledgers`][get-ledgers-endpoint].

The Ledger ID does not (in general) change, so you may store it in configuration
in the same way that you store the MSN.
We may add display of the Ledger ID on
[portal.vipps.no](https://portal.vipps.no)
in the future.

## What text is shown for the payouts in my bank?

We send text on this format to all banks:
`Utb. <settlement_number> Vippsnr <serial_no>`.

Example: `Utb. 2000810 Vippsnr 117703`.

We have no control over, or information about, how banks handle this on their side,
or how it is displayed in their reports online or in print.

The file format used to transfer information to the bank is the
[ISO 20022](https://www.iso20022.org)
PAIN (PAyment INitiation) format, which - among other things - has a limit of 35
characters for the payment text.

## How can I get the details for a payment I see in my bank?

**Please note:** The payout from Vipps MobilePay to the merchant is
the balance at the time of the payout. The sum in the payout, which is what
the merchant sees in the bank, does not contain specific payments -
it is simply the balance at the time the payout was made.

The payout details can be found with these endpoints, where `{topic}` is `funds`:

* [`GET:/report/v2/ledgers/{ledgerId}/{topic}/dates/{ledgerDate}`][fetch-report-by-date-endpoint]
* [`GET:/report/v2/ledgers/{ledgerId}/{topic}/feed`][fetch-report-by-feed-endpoint]

Match the settlement ID in the bank with `pspReference`.

The Report API is designed to provide updated data independently of the bank payments.
There are several reasons for this, including:

1. A merchant can have a negative balance and not get settlement payouts for a long time.
   Periodic checks of settlements would thus not make sense until the merchant has a positive again.
1. Merchants have different risk profiles (or could have at least). If we deemed that we would hold
   back some of the balance from payouts to an airline company.
   Let's say they sell three tickets at 5000 NOK each. We then change their risk profile saying we
   need to hold back 7500 NOK and then the settlement run. We will then pay out 7500 NOK
   (if there are no fees). Now: What captures will then be part of that settlement?
1. We may, in rare cases, manually correct payouts.

## How can I get the data for VM-number sales units with shopping basket?

This is only available on
[portal.vipps.no](https://portal.vipps.no),
but we may extend the Report API top include more details.
There are no specific plans to do this yet.

## How can I get the details for my shopping basket products and their different VAT rates?

This is only available on
[portal.vipps.no](https://portal.vipps.no),
but we may extend the Report API top include more details.
There are no specific plans to do this yet.

## How can I get details about my customers' tax deductions?

This is used for fundraising, where users may give consent to share their
national identity number and automatically get tax deductions.

This is only available on
[portal.vipps.no](https://portal.vipps.no),
but we may extend the Report API top include more details.
There are no specific plans to do this yet.

## How can I get the details for each payment?

The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.

The API used to initiate the payment may be used to get the details
for the payment. See:
- eCom API: [Get payment details]](https://developer.vippsmobilepay.com/docs/APIs/ecom-api/vipps-ecom-api/#get-payment-details)
- ePayment API: [Get payment](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/get_info/)
  and [Get payment event log](https://developer.vippsmobilepay.com/docs/APIs/epayment-api/operations/get_event_log/)
- Recurring API: [Retrieve a charge](https://developer.vippsmobilepay.com/docs/APIs/recurring-api/recurring-api-guide/#retrieve-a-charge)

If you use the
[Order Management API](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/)
you can retrieve all details from there too:
[Fetch Category and Receipt](https://developer.vippsmobilepay.com/docs/APIs/order-management-api/order-management-api-guide/#fetching-category-and-receipt).
This includes all the receipt details with all the order lines.

The Report API _may_ be extended to contain more information later,
and this FAQ will be updated if there are any changes.
There are no specific plans to do this yet.

## How do I get the settlements for multiple MSNs in the same ledger?

Joint settlement for multiple MSNs is supported, and may be relevant
in cases such as:

* You are changing the technical integration and getting a new MSN
  for this purpose, but you do not want to disrupt the settlement
  series.
* You are launching a Point of Sale system in many physical distinct
  stores that should have different MSNs, but want to have
  combined settlements for these.

In practice, this feature is little used in a few pilot cases,
and configuration is not yet generally available.
However, accounting partners should take into account that this feature
*could* see more use in the future when integrations are developed.

## If a ledger is used for two MSN: MSN 1 and MSN 2. What happens when MSN 2 gets its own ledger?

Once a transaction has appeared on a ledger, it belongs to that ledger forever.

When an MSN is moved to another ledger, it means that *future* transactions
will appear on the new ledger. The old transactions appear on the old ledger,
since they contributed to joint settlement payouts of both MSN 1 and MSN 2.

## Where can I find the settlement ID?

The `payout` corresponds to the `SettlementID` from the
[XML](https://developer.vippsmobilepay.com/docs/settlements/#xmlsettlements/xml)
files in the
[SFTP report (deprecated)](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

## How can a merchant or partner get access to the API?

See:
[Authenticating to the Report API](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/).

## Which API keys give access to the API?

See:
[Authenticating to the Report API](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/).

## Why do I get an empty list in response when calling one of the endpoints?

If you keep getting an empty list in a response where you expect data to be available, you should first check these things before filing an issue:

* Check if your query-parameters is correct. Are you filtering on the correct date?
* Do you have access to the resource you are trying to retrieve? If you are trying to retrieve the transactions connected to a ledger you do not have access to, you will be given an empty list


[get-ledgers-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/settlementv1/operation/getLedgers
[fetch-report-by-date-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1dates~1%7BledgerDate%7D/get
[fetch-report-by-feed-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1feed/get
