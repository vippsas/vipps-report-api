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

For more common questions, see:

* [API General FAQ](https://developer.vippsmobilepay.com/docs/faqs)


## What are the benefits of the Report API over the SFTP service?

The
[SFTP Report Service](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service)
lets merchants and partners download settlement files by SFTP.

The Report API has some benefits over this:

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

## What information can I get hold of?

Right now the only data that is available is the same data that is already
available in the settlement reports on
[portal.vipps.no](https://portal.vipps.no),
or on
[SFTP](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

The only difference is that the data can be fetched over a more modern REST API.
We aim to provide more information through this API in the future.

**Important:** It is not yet possible to use the Report API to retrieve data
for *Vippsnummer* sale units. The Report API can only be used to sale units
that have API access (which *Vippsnummer* sale units do not have.)

## Is the Report API available for the test environment?

Unfortunately not because the
[test environment](https://developer.vippsmobilepay.com/docs/test-environment)
does not have the backend systems necessary to offer the Report API.

The only way to test the Report API is in the production environment.

Since the Report API is read-only, and the overall logic is very simple
(first ask for data, then receive that data), developing and testing in
the production environment is straight-forward.

Implementing the Report API in the test environment is a big effort,
as there are no settlements being made in that environment. The data that
the Report API needs is simply not available.
It is unlikely that we'll be able to prioritize this over other development efforts.

## Can a merchant find the Ledger ID for an MSN on portal.vipps.no?

This is not possibly currently.

For now, the only way to get the Ledger ID is to call
[`GET:settlement/v1/ledgers`][get-ledgers-endpoint].

The Ledger ID does not in general change, so you may store it in configuration
in the same way that you store the MSN.
We may add display of the Ledger ID on
[portal.vipps.no](https://portal.vipps.no)
in the future.

## How can I get the details for a payment I see in my bank?

**Please note:** The payout from Vipps MobilePay to the merchant is
the balance at the time of the payout. The sum in the payont, which is what
the merchant sees in the bank, does not contain specific payments -
it is simply the balance at the time the payout was made.

The payout details can be found with the `/funds` endpoint, either under date or
using the feed. Match the settlement id in the bank with `pspReference`.

The Report API is designed to provide updated data independently of the bank payments. There are several reasons for this, including:

1. A merchant can have a negative balance and not get settlement payouts for a long time.
   Periodic checks of settlements would thus not make sense until the merchant has a positive again.
1. Merchants have different risk profiles (or could have at least). If we deemed that we would hold
   back some of the balance from payouts to an airline company.
   Let's say they sell three tickets at 5000 NOK each. We then change their risk profile saying we
   need to hold back 7500 NOK and then the settlement run. We will then pay out 7500 NOK
   (if there are no fees). Now: What captures will then be part of that settlement?
1. We may, in rare cases, manually correct payouts. 

## How can I get the data for VM-number with shopping basket?

This is only available on
[portal.vipps.no](https://portal.vipps.no),
but we may extend the Report API top include more details.
There are no specific plans to do this yet.

## How can I get the details for my shopping basket product nad their different VAT rates?

This is only available on
[portal.vipps.no](https://portal.vipps.no),
but we may extend the Report API top include more details.
There are no specific plans to do this yet.

## How do I get the settlements for multiple MSNs in the same ledger?

Joint settlement for multiple MSNs is supported, and may be relevant
in cases such as:

* You are changing the technical integration and getting a new MSN
  for this purpose, but you do not want to disrupt the settlement
  series
* You are launching a Point of Sale system in many physical distinct
  stores that should have different MSNs, but want to have
  combined settlements for these.

In practice, this feature is little used in a few pilot cases, and
configuration is not yet generally available.
However, accounting partners should take into account that this feature
*could* see more use in the future when integrations are developed.

## If a ledger is used for two MSN: MSN 1 and MSN 2. What happens when MSN 2 gets its own ledger?

Once a transaction has appeared on a ledger, it belongs to that ledger forever.
When an MSN is moved to another ledger, it means that *future* transactions
will appear on the new ledger. The old transactions appear on the old ledger,
since they contributed to joint settlement payouts of both MSN 1 and MSN 2.

## Where can I find the settlement ID?

The "payout" corresponds to the `SettlementID` from the
[XML](https://developer.vippsmobilepay.com/docs/settlements/xml)
files in the
[SFTP report service](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

## How can a merchant get access to the API?

The API is available for sales units that already have access to any other Vipps MobilePay API.

If you do not already have API access, you cannot get access to the Report API.
This is the case if you only have a *Vippsnummer* sales unit, since those
sales units have no API access.

## How can an accounting partner get access to the API?

Accounting partners must use their
[accounting keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys/#types-of-partner-keys).

**Important:** Merchants are not allowed to share API keys with partners that have not been approved by
Vipps MobilePay, as we are strictly regulated and must know who can make payments using our APIs.

Accounting companies can use the
[form on vipps.no](https://www.vipps.no/developer/become-a-partner/)
to become an accounting partner.

Merchants select their accounting partner on portal.vipps.no as described here:
[Give access to an accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner).

## Which API keys give access to the API?

The following API keys give access to the Report API:

* The merchant's own API keys: The same API keys that are used to make payments, etc. See
  [API keys](https://developer.vippsmobilepay.com/docs/common-topics/api-keys).
  Only the merchant can use these API keys with the Report API.

In the future, *approximately Q4 2023*,
[Accounting keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys) will provide access.
The API keys provided to the accounting partner
when the partner signed a contract with Vipps MobilePay. The accounting partner's
API keys only work for sales units after the merchant has
[given access to the accounting partner](./api-guide/overview.md#give-access-to-an-accounting-partner).


### The merchant's own API keys

The merchant's own API keys give full access to the Report API.

If the merchant uses an integration partner (see
[partner types](https://developer.vippsmobilepay.com/docs/partner#partner-types)),
it is the same as using the merchant's own API keys.

When a merchant shares its API keys for a sales unit with an integration partner,
we have no way of knowing whether the API calls are made by the merchant or
the integration partner.
This is why we require that all API requests made by a partner on behalf of a
merchant are done using the partner's own API keys (the accounting keys).

### Specifying an accounting partner

In addition to the above, the merchant may add one or more accounting partners.
An accounting partner will get access to the Report API, but will not be allowed
to make payments, or move money in any way. The accounting partner can only see
reports of the payments that have been made.

See:
[Give access to an accounting partner](./api-guide/overview.md#give-access-to-an-accounting-partner)

## Why do I get an empty list in response when calling one of the endpoints?

If you keep getting an empty list in a response where you expect data to be available, you should first check these things before filing an issue:

* Check if your query-parameters is correct. Are you filtering on the correct date?
* Do you have access to the resource you are trying to retrieve? If you are trying to retrieve the transactions connected to a ledger you do not have access to, you will be given an empty list


[get-ledgers-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/settlementv1/operation/getLedgers
[fetch-report-by-date-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1dates~1%7BledgerDate%7D/get
[fetch-report-by-feed-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1feed/get
