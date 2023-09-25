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
[Vipps Report API guide](./api-guide/README.md)
for all the details.

For more common questions, see:

* [API General FAQ](https://developer.vippsmobilepay.com/docs/faqs)

## What is the status of the API?

The Report API is currently in public beta.
It is unlikely to change, but
there may be smaller changes made still. We expect a final freeze of the
API before summer 2023.

Currently, we only support single-MSN API keys (see the question below
on API keys for more information on future plans for partner keys).

## What are the benefits of the Report API over the SFTP service?

The
[Vipps SFTP Report Service](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service)
lets merchants and partners download settlement files by SFTP.

The Report API has some benefits over this:

* Data can be retrieved at any time.
* Data is available regardless of whether the balance is positive or negative,
  so even if there have only been refunds, there will be data available.
* The SFTP service only provides settlement files if the balance is positive.
* It is possible to retrieve data for more than one day in a request,
  so you could, for example, retrieve an entire weekend, week, or month.
* The SFTP service only provides one file per day.
* The data is in JSON format, making it easy to use in different ways.

Later:

* Support for
  [Partner keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys),
  so an accounting partner can use its API keys for all its merchants.
* Functionality on
  [portal.vipps.no](https://portal.vipps.no).
  for merchants to
  [give access to an accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview#give-access-to-an-accounting-partner).

## What information can I get hold of?

Right now the only data that is available is the same data that is already
available in the settlement reports on
[portal.vipps.no](https://portal.vipps.no),
or on
[SFTP](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

The only difference is that the data can be fetched over a more modern REST API.
We aim to provide more information through this API in the future.

**Important:** It is not yet possible to use the Report API to retrieve data
for "Vippsnummer" sale units. The Report API can only be used to sale units
that have API access (which "Vippsnummer" sale units do not have.)

## Is the Report API available for the test environment?

Nope. The
[Vipps test environment](https://developer.vippsmobilepay.com/docs/test-environment)
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

No, not currently.

For now,
the only way to get the Ledger ID is to call
[`GET:settlement/v1/ledgers`](https://developer.vippsmobilepay.com/api/report#/paths/~1settlement~1v1~1ledgers/get).

The Ledger ID does not in general change, so you may store it in configuration
in the same way that you store the MSN.
We may add display of the Ledger ID on
[portal.vipps.no](https://portal.vipps.no)
in the future.

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

## How can I see which payment a payment is included in?

You can use the `inPayout` query parameter for the
[`GET:/report/v1/ledgertransactions?ledgerId={ledgerId}`](https://developer.vippsmobilepay.com/api/report#/paths/~1report~1v1~1ledgertransactions?ledgerId=%7BledgerId%7D/get)
endpoint to retrieve payments that are part of a specific payout.
The `sincePayout` works similarly.

**Please note:** Vipps may at some point implement a "rolling reserve", where
an amount is always withheld to compensate for high risk, etc. Due to this, each
payment may not always be connected to a specific payout.

## Where can I find the settlement ID?

The "payout" corresponds to the `SettlementID` from the
[XML](https://developer.vippsmobilepay.com/docs/settlements/xml)
files in the
[SFTP report service](https://developer.vippsmobilepay.com/docs/settlements/sftp-report-service).

## How can I find the value to use with `inPayout`?

The value for `inPayout` is a sequential number, starting with `2000001`, and
incrementing by one for every payout.

You can find the latest number in the sequence used so far from
[`GET:/settlement/v1/ledgers`](https://developer.vippsmobilepay.com/api/report#/paths/~1settlement~1v1~1ledgers/get).
Each Ledger has both the first and last payout used on it:
`firstPayout` and `lastPayout`.

## How can I get access to the API?

The API is available for sales units that already have access to a
[Vipps API](https://developer.vippsmobilepay.com/docs/APIs).

If you do not already have API access, you cannot get access to the Report API.
This is the case if you only have a "Vippsnummer" sales unit, since those
sales units have no API access.

## Which API keys give access to the API?

For now: The following API keys give access to the Report API:

* The merchant's own API keys: The same API keys that are used to make payments, etc. See
  [API keys](https://developer.vippsmobilepay.com/docs/common-topics/api-keys).

Later (we can not give an ETA, but will update here):

* [Partner keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys):
  The API keys that are used by partners to make payments on behalf of their merchants.
* Accounting partner's API keys: The API keys provided to the accounting partner
  when the partner signed a contract with Vipps. The accounting partner's
  API keys only work for sales units after the merchant has
  [given access to the accounting partner](./api-guide/overview.md#give-access-to-an-accounting-partner).

### The merchant's own API keys

The merchant's own API keys give full access to the Report API.

If the merchant uses an integration partner (see
[partner types](https://developer.vippsmobilepay.com/docs/partner#partner-types)),
it is the same as using the merchant's own API keys.

When a merchant shares its API keys for a sales unit with an integration partner,
Vipps has no way of knowing whether the API calls are made by the merchant or
the integration partner.
This is why we require that all API requests made by a partner on behalf of a
merchant are done using the partner's own API keys (the partner keys).

### Partner keys

**Please note:**
[Partner keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys)
are not yet supported by the Report API,
only [the merchant's own API keys](#the-merchants-own-api-keys)
can be used.
This notice will be removed when partner keys are supported.
Subscribe to the
[Technical newsletter](https://developer.vippsmobilepay.com/docs/newsletters)
for updates.

Partner keys let a partner make payments on behalf of a merchant, and the same API keys
also give access to the Report API.

If the merchant has a sales unit that is configured with a
[platform partner](https://developer.vippsmobilepay.com/docs/partner#partner-types),
the platform partner can use
[partner keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys)
to make requests to the Report API.

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
