---
title: Overview
sidebar_position: 31
sidebar_label: Overview
pagination_prev: Null
pagination_next: Null
---

# Overview

**Please note:** The Report API is mainly for accounting partners: They will use the API to integrate
with their accounting systems, enabling their merchants to log in on
[portal.vipps.no](https://portal.vipps.no)
and
[give access to the accounting partner](https://developer.vippsmobilepay.com/docs/APIs/report-api/api-guide/overview/#give-access-to-an-accounting-partner)
(without the merchant doing any development).
Merchants with API access (and API keys) may integrate with the Report API
directly, but since all merchants should have an accounting system, such integration should not be necessary.

## Authenticating to the Report API

The Report API is available for sales units that have access to the Vipps MobilePay API platform.
If you are new to the platform, see [Getting started](https://developer.vippsmobilepay.com/docs/getting-started).

Access to the Report API is provided through the merchant's API keys which grant access to a single Merchant Serial Number (MSN).

Individual merchants may use their API keys to access a single ledger connected to the sales unit (identified with MSN).
A ledger may have multiple sales unit associated; however, access will only be provided to the ledger connected to the MSN.

Note, *Vippsnummer* sales units don't have API access and thus have no availability to use this API.

## Planned accounting partner functionality

(*Available approximately Q4 2023*)

We are currently working on providing partners access to the API,
and such support will be launched in the future.
There is a second planned way to connect to the Report API:

* Using an Accounting partner's API keys


### Give access to an accounting partner

(*Available approximately Q4 2023*)

Merchants must give access to their accounting partner on
[portal.vipps.no](https://portal.vipps.no).

### Overview of accounting partners

(*Available approximately Q4 2023*)

A merchant may have zero or more accounting partners. This page on
[portal.vipps.no](https://portal.vipps.no)
shows the accounting partners for one sales unit.

![Overview over accounting-partners](../images/portal-accounting-partners-overview.png "Accounting Partners overview")

### Adding a new accounting partner

(*Available approximately Q4 2023*)

This page on
[portal.vipps.no](https://portal.vipps.no)
shows how to add an accounting partners, and how to specify which ledgers the
accounting partner will have access to.

![Add a new accounting-partner](../images/portal-accounting-partners-edit.png "Add a new accounting partner")
<!-- END_COMMENT -->