<!-- START_METADATA
---
title: FAQ
sidebar_position: 60
pagination_prev: Null
pagination_next: Null
---
END_METADATA -->

# Frequently asked questions

Here are the Report API FAQs.
See the
[Vipps Report API guide](./api-guide/README.md)
for all the details.

For more common Vipps questions, see:

* [Vipps API General FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs)

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/docs/APIs/report-api).

## Table of contents

* [What is the status of the API?](#what-is-the-status-of-the-api)
* [What information can I get hold of?](#what-information-can-i-get-hold-of)
* [Is the Report API available for the test environment.](#is-the-report-api-available-for-the-test-environment)
* [Can a merchant find the Ledger ID for a MSN on portal.vipps.no?](#can-a-merchant-find-the-ledger-id-for-a-msn-on-portalvippsno)
* [How do I get the settlements for multiple MSNs in the same ledger?](#how-do-i-get-the-settlements-for-multiple-msns-in-the-same-ledger)
* [If a ledger is used for two MSN: MSN 1 and MSN 2. What happens when MSN 2 gets its own ledger?](#if-a-ledger-is-used-for-two-msn-msn-1-and-msn-2-what-happens-when-msn-2-gets-its-own-ledger)
* [How can I see which payment a payment is included in?](#how-can-i-see-which-payment-a-payment-is-included-in)
* [Which API keys give access to the API?](#which-api-keys-give-access-to-the-api)
  * [The merchant's own API keys](#the-merchants-own-api-keys)
  * [Partner keys](#partner-keys)
  * [Specifying an accounting partner](#specifying-an-accounting-partner)
* [Why do I get an empty list in response when calling one of the endpoints?](#why-do-i-get-an-empty-list-in-response-when-calling-one-of-the-endpoints)

<!-- END_COMMENT -->

## What is the status of the API?

The Report API is currently in public beta.
It is unlikely to change, but
there may be smaller changes made still. We expect a final freeze of the
API before summer 2023.

Currently we only support single-MSN API keys (see the question below
on API keys for more information on future plans for partner keys).

## What information can I get hold of?

Right now the only data that is available is the same data that is already
available in the settlement reports on
[portal.vipps.no](https://portal.vipps.no),
or on
[SFTP](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements/sftp-report-service).

The only difference is that the data can be fetched over a more modern REST API.
We aim to provide more information through this API in the future.

## Is the Report API available for the test environment.

Nope. The
[Vipps test environment](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/test-environment)
does not have the backend systems necessary to offer the Report API.
The only way to test the Report API is in the production environment.

## Can a merchant find the Ledger ID for a MSN on portal.vipps.no?

No, not currently. For now,
the only way to get the Ledger ID is to call
[`GET:settlement/v1/ledgers`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1settlement~1v1~1ledgers/get).
The Ledger ID does not in general change, so you may store it in configuration
in the same way that you store the MSN.
We may add display of the Ledger ID on portal.vipps.no in the future.

## How do I get the settlements for multiple MSNs in the same ledger?

Joint settlement for multiple MSNs is supported, and may be relevant
in cases such as:
* You are changing the technical integration and getting a new MSN
  for this purpose, but you do not want to disrupt the settlement
  series
* You are launching a Point of Sale system in a large number of distinct
  physical stores that should have different MSNs, but want to have
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
[`GET:/report/v1/ledgertransactions?ledgerId={ledgerId}`](https://vippsas.github.io/vipps-developer-docs/api/report#/paths/~1report~1v1~1ledgertransactions?ledgerId=%7BledgerId%7D/get)
endpoint to retrieve payments that are part of a specific payout.
The `sincePayout` works in a similar way.

**Please note:** Vipps may at some point implement a "rolling reserve", where
an amount is always withheld to compensate for high risk, etc. Due to this, each
payment may not always be connected to a specific payout.

## Which API keys give access to the API?

For now: The following API keys give access to the Report API:

* The merchant's own API keys: The same API keys that are used to make payments.

Later (we can not give an ETA, but will update here):

* Partner keys: The same API keys that are used to make payments.
* Accounting partner's API keys: The API keys provided to the accounting partner
  when the partner signed a contract with Vipps. The accounting partner's
  API keys only work for sale units after the merchant has
  [given access to the accounting partner](./api-guide/overview.md#give-access-to-an-accounting-partner).

### The merchant's own API keys

The merchant's own API keys give full access to the Report API.

If the merchant uses an integration partner (see
[partner types](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/#partner-types)),
it is the same as using the merchant's own API keys.

When a merchant shares its API keys for a sale unit with an integration partner,
Vipps has no way of knowing whether the API calls are made by the merchant or
the integration partner.
This is why we require that all API requests made by a partner on behalf of a
merchant are done using the partner's own API keys (the partner keys).

### Partner keys

Partner keys let a partner make payments on behalf of a merchant, and the same API keys
also give access to the Report API.

If the merchant has a sale unit that is configured with a
[platform partner](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/#partner-types),
the platform partner can use
[partner keys](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/partner-keys)
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
