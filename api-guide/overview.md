---
title: Overview
sidebar_position: 31
sidebar_label: Overview
pagination_prev: Null
pagination_next: Null
---

# Overview

The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.
Merchants can then simply
[give access to the accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner),
without doing any development themselves.


## Authenticating to the Report API

The Report API is available for sales units that have access to the Vipps MobilePay API platform.
If you are new to the platform, see [Getting started](https://developer.vippsmobilepay.com/docs/getting-started).

Access to the Report API is provided through the merchant's API keys which grant access to a single Merchant Serial Number (MSN).

Individual merchants may use their API keys to access a single ledger connected to the sales unit (identified with MSN).
A ledger may have multiple sales unit associated; however, access will only be provided to the ledger connected to the MSN.

Note, *Vippsnummer* sales units don't have API access and thus have no availability to use this API.

## Planned accounting partner functionality

(*Available approximately Q4 2023*)

We are currently working on providing accounting partners access to the API.
There is a second planned way to connect to the Report API:

* Using an Accounting partner's API keys

A merchant can have any number accounting partners. The merchant portal
shows the accounting partners for each sales unit.
Merchants must add accounting partners and give them access to a sales unit through the
[merchant portal](https://portal.vipps.no).

![Overview over accounting-partners](../images/portal-accounting-partners-overview.png)

Merchants must specify which ledgers the accounting partner will have access to.

![Add a new accounting-partner](../images/portal-accounting-partners-edit.png)
