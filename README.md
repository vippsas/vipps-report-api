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

![Vipps](./images/vipps.png) *Version 1 is available for Vipps. Version 2 is expected in early October 2023.*

![MobilePay](./images/mp.png) *Available for MobilePay in selected markets at the [Vipps MobilePay joint platform launch](https://www.vippsmobilepay.com/#about).*

**Please note:** The Report API is mainly for accounting partners: They will use the API to integrate
with their accounting systems, enabling their merchants to log in on
[portal.vipps.no](https://portal.vipps.no)
and
[give access to the accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner)
(without the merchant doing any development).
Merchants with API access (and API keys) may integrate with the Report API
directly, but since all merchants should have an accounting system, such integration should not be necessary.

## Report API documentation

* [API quick start](vipps-report-api-quick-start.md): Run the basic examples in curl and Postman.
* [API guide](./api-guide/README.md): Learn about the Report API features.
* [API FAQ](vipps-report-api-faq.md): Look for your question among those people have asked before.
* [API reference](https://developer.vippsmobilepay.com/api/report): Go straight to the endpoint specifications.

If you're new to the platform, see
[Getting started](https://developer.vippsmobilepay.com/docs/getting-started/)
for information about API keys, product activation, and the test environment.

## Versions

* This documents version 2 of the API, `report/v2`. The documentation for `report/v1`
  is available on GitHub at <https://github.com/vippsas/vipps-report-api/tree/v1>.
* As of this writing, only version 1 is available in production. The version 1
  is considered a *beta version* and support will be removed within some months.
* Version 2 of the API will be available within some weeks.

Feedback? Great! You can
[submit an issue on GitHub](https://github.com/vippsas/vipps-report-api/issues).
