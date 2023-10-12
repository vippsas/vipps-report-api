---
title: Authenticating to the Report API
sidebar_position: 31
sidebar_label: Authentication
pagination_prev: Null
pagination_next: Null
---


# Authenticating to the Report API

![Vipps](../images/vipps.png) *Version 1 is deprecated. Version 2 is expected in Q4 2023.*

The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.
Merchants can then simply
give access to the accounting partner,
without doing any development themselves.
**Please note:**
The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.
Merchants can then simply
[give access to the accounting partner](#give-access-to-an-accounting-partner),
without doing any development themselves.
*This will be available in Q4 2023.
See
[Planned accounting partner functionality](#planned-accounting-partner-functionality) for more details.*


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

### Give access to an accounting partner

A merchant can have any number accounting partners. The merchant portal
shows the accounting partners for each sales unit.
Merchants must add accounting partners and give them access to a sales unit through the
[merchant portal](https://portal.vipps.no).

![Overview over accounting-partners](../images/portal-accounting-partners-overview.png)

Merchants must specify which ledgers the accounting partner will have access to.

![Add a new accounting-partner](../images/portal-accounting-partners-edit.png)
