<!-- START_METADATA
---
title: FAQ
sidebar_position: 60
pagination_next: null
---
END_METADATA -->

# Vipps Report API: Frequently Asked Questions

Here are the Report API FAQs.
See the
[Vipps Report API guide](vipps-report-api.md)
for all the details.

For more common Vipps questions, see:

* [Vipps API General FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs)

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

## Table of contents

* [When will the API be available?](#when-will-the-api-be-available)
* [What information can I get hold of?](#what-information-can-i-get-hold-of)
* [Which API keys give access to the API?](#which-api-keys-give-access-to-the-api)
  * [The merchant's own API keys](#the-merchants-own-api-keys)
  * [Partner keys](#partner-keys)
  * [Specifying a accounting partner](#specifying-a-accounting-partner)
  * [I keep getting an empty list in response when calling one of the endpoints](#i-keep-getting-an-empty-list-in-response-when-calling-one-of-the-endpoints)

<!-- END_COMMENT -->

## When will the API be available?

The Vipps Report API will be available from January 9 2023.

## What information can I get hold of?

Right now the only data that is available is the same data that is already
available in the settlement reports on
[portal.vipps.no](https://portal.vipps.no),
or on
[SFTP](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/settlements/sftp-report-service).

The only difference is that the data can be fetched over a more modern REST API.
We aim to provide more information through this API in the future.

## Which API keys give access to the API?

For now: The following API keys give access to the Report API:

* The merchant's own API keys: The same API keys that are used to make payments.

Later (we can not give an ETA, but will update here):

* Partner keys: The same API keys that are used to make payments.
* Accounting partner's API keys: The API keys provided to the accounting partner
  when the partner signed a contract with Vipps. The accounting partner's
  API keys only work for sale units after the merchant has
  [given access to the accounting partner](vipps-report-api.md#give-access-to-an-accounting-partner).

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

### Specifying a accounting partner

In addition to the above, the merchant may add one or more accounting partners.
An accounting partner will get access to the Report API, but will not be allowed
to make payments, or move money in any way. The accounting partner can only see
reports of the payments that have been made.

See:
[Give access to an accounting partner](vipps-report-api.md#give-access-to-an-accounting-partner)

### I keep getting an empty list in response when calling one of the endpoints

If you keep getting an empty list in a response where you expect data to be available, you should first check these things before filing an issue:

* Check if your query-parameters is correct. Are you filtering on the correct date?
* Do you have access to the resource you are trying to retrieve? If you are trying to retrieve the transactions connected to a ledger you do not have access to, you will be given an empty list
