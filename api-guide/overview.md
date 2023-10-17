---
title: Authenticating to the Report API
sidebar_position: 31
sidebar_label: Authentication
pagination_prev: Null
pagination_next: Null
---


# Authenticating to the Report API

![Vipps](../images/vipps.png) *Version 1 is deprecated. Version 2 is expected in October 2023.*

**Please note:**
The Report API is primarily for accounting partners who will use the API to integrate
with their accounting systems, allowing them to provide the accounting information to their merchants.
Accounting partners use their
[accounting keys](https://developer.vippsmobilepay.com/docs/partner/partner-keys/),
and are not allowed to use the merchant's own API keys.
Merchants can then simply
[give access to the accounting partner](#give-access-to-an-accounting-partner),
without doing any development themselves.

## Sales units that have API access

The Report API is available for sales units that have access to the Vipps MobilePay API platform.
If you are new to the platform, see
[Getting started](https://developer.vippsmobilepay.com/docs/getting-started).

Access to the Report API is provided through the merchant's API keys for the sales unit,
which grant access to that single Merchant Serial Number (MSN).

Individual merchants may use their API keys to access a single ledger connected to the sales unit (identified with MSN).
A ledger may have multiple sales unit associated; however, access will only be provided to the ledger connected to the MSN.

## Sales units that do not have API access

Vippsnummer sales units don't have API access and there is no way for a merchant to
access the API using its own API keys - there are none.

Vippsnummer sales units can only use the Report API through an accounting partner.

### Give access to an accounting partner

A merchant can have any number accounting partners: One accounting partner for each sales unit.
A sales unit can only have one accounting partner.

The merchant portal shows the accounting partners for each sales unit.
Merchants must add accounting partners and give them access to a sales unit on
[portal.vipps.no](https://portal.vipps.no).

![Overview over accounting-partners](../images/portal-accounting-partners-overview.png)

Merchants must specify which ledgers the accounting partner will have access to.

![Add a new accounting-partner](../images/portal-accounting-partners-edit.png)
