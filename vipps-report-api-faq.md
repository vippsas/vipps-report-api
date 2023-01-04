<!-- START_METADATA
---
title: FAQ
sidebar_position: 60
pagination_next: null
---
END_METADATA -->

# Vipps Report API: Frequently Asked Questions

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

Here are the Report API FAQs.
See the
[Vipps Report API guide](vipps-report-api.md)
for all the details.

For more common Vipps questions, see:

* [Vipps API General FAQ](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/faqs/)

Document version: 0.0.6.

<!-- START_TOC -->

## Table of contents

* [When will the API be available?](#when-will-the-api-be-available)
* [Which API keys give access to the API?](#which-api-keys-give-access-to-the-api)
  * [The merchant's own API keys](#the-merchants-own-api-keys)
  * [Partner keys](#partner-keys)
  * [Specifying a accounting partner](#specifying-a-accounting-partner)
* [Questions?](#questions)

<!-- END_TOC -->

## When will the API be available?

We can not give a date, month or any kind of ETA yet.

We are still quite early in the planning and design phase and will use the
draft documentation here to discuss with potential users of the API
and make sure we are developing the right solution.

## Which API keys give access to the API?

In short: The following API keys give access to the Report API:

* The merchant's own API keys: The same API keys that are used to make payments.
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


## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-report-api/issues),
a [pull request](https://github.com/vippsas/vipps-report-api/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).
