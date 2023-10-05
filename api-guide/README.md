---
title: Introduction
sidebar_label: API guide
sidebar_position: 30
pagination_prev: Null
pagination_next: Null
---

# API guide

![Vipps](../images/vipps.png) *Version 1 is available for Vipps. Version 2 is expected in early October 2023.*

![MobilePay](../images/mp.png) *Available for MobilePay in selected markets at the [Vipps MobilePay joint platform launch](https://www.vippsmobilepay.com/#about).*

**Please note:** The Report API is mainly for accounting partners: They will use the API to integrate
with their accounting systems, enabling their merchants to log in on
[portal.vipps.no](https://portal.vipps.no)
and
[give access to the accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner)
(without the merchant doing any development).
Merchants with API access (and API keys) may integrate with the Report API
directly, but since all merchants should have an accounting system, such integration should not be necessary.

The Report API can provide you with information about your payments with Vipps MobilePay. In particular, it is used to
aggregate information across the API platform, and it can contain data for many
payments at once.

The following topics are described:

* [Overview](overview.md) contains descriptions of common traits of the entire Report API.
* [Settlement guide](settlement.md) explains the Report API settlement process and the ledger.
* [Errors description](errors.md) shows the Report API errors with descriptions.
* Coming later: Endpoints for recent/non-settled transactions.

**Please note:** The information fetched from the Report API is
asynchronous and trailing behind the other APIs. It is usually behind
by a second or so, but if there are operational problems, it could, in the worst
case, be behind by several hours. Therefore, you should always use other
Vipps APIs as the source of truth for the status of an operation.
