---
title: Introduction
sidebar_label: API guide
sidebar_position: 30
pagination_prev: Null
pagination_next: Null
---

# API guide

The Report API can provide you with information about your payments with Vipps. In particular, it is used to
aggregate information across the Vipps APIs and it can contain data for many
payments at once.

The following topics are described:

- [Overview](overview.md) contains descriptions of common traits of the entire Report API.
- [Settlement guide](settlement.md) explains the Report API settlement process and the ledger.
- [Errors description](errors.md) shows the Report API errors with descriptions.
- Coming later: Endpoints for recent/non-settled transactions.

**Please note:** The information fetched from the Report API is
asynchronous and trailing behind the other APIs. It is usually behind
by a second or so, but if there are operational problems, it could, in the worst
case, be behind by several hours. Therefore, you should always use other
Vipps APIs as the source of truth for the status of an operation.

