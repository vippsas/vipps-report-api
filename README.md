---
title: Introduction to the Report API
sidebar_label: Introduction
sidebar_position: 1
hide_table_of_contents: true
description: Use the Report API to fetch information about payment events.
pagination_next: null
pagination_prev: null
---

# Report API

![Vipps](./images/vipps.png) *Version 2 is available now. Version 1 is deprecated.*

![MobilePay](./images/mp.png) *Available for MobilePay in selected markets at the [Vipps MobilePay joint platform launch](https://www.vippsmobilepay.com/about).*

**Please note:**
The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.
Accounting partners use their
[accounting keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys/),
and are not allowed to use the merchant's own API keys.
Merchants can then simply
[give access to the accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner),
without doing any development themselves.

## Report API documentation

* [API quick start](report-api-quick-start.md): Run the basic examples in curl and Postman.
* [API guide](./api-guide/README.md): Learn about the Report API features.
* [API FAQ](report-api-faq.md): Look for your question among those people have asked before.
* [API reference](https://developer.vippsmobilepay.com/api/report): Go straight to the endpoint specifications.

If you're new to the platform, see
[Getting started](https://developer.vippsmobilepay.com/docs/getting-started/)
for information about API keys, product activation, and the test environment.

## Versions

* The API guide is updated for Report API version 2, `report/v2`. The `report/v1` endpoint is deprecated,
  but viewable as part of the [API spec](https://developer.vippsmobilepay.com/api/report/).
* As of now, only version 1 is available in production. The version 1
  is considered a *beta version* and support will be removed within some months.
* Version 2 of the API will be available within some weeks.

Feedback? Great! You can
[submit an issue on GitHub](https://github.com/vippsas/vipps-report-api/issues) or in the feedback field below.
