---
title: Overview
sidebar_position: 31
sidebar_label: Overview
pagination_prev: Null
pagination_next: Null
---

# Overview

## Authenticating to the Report API

Currently, access to the Report API is only provided through
using the merchant's API key that grant access to a single MSN.
See
[Which API keys give access to the API?](../vipps-report-api-faq.md#which-api-keys-give-access-to-the-api)
for details about these.

See:

* [How can I get access to the API?](../vipps-report-api-faq.md#how-can-i-get-access-to-the-api)
* [Getting started: Get an access token](https://developer.vippsmobilepay.com/docs/vipps-developers/getting-started#get-an-access-token)

### Planned accounting partner functionality

(DRAFT)

We are currently working on providing partners access to the API,
and such support will be launched in the future.
There is a second planned way to connect to the Report API:

* Using an Accounting partner's API keys

### Using the merchant's API keys

Individual merchants that have API keys
for using the eCom/ePayment APIs may use this to access a single
ledger connected to the sales unit (identified with MSN). If there
is more than one sales unit connected to the ledger, access will be denied.

## Give access to an accounting partner

(DRAFT)

Merchants must give access to their accounting partner on
[portal.vipps.no](https://portal.vipps.no).

### Overview of accounting partners

(DRAFT)

A merchant may have zero or more accounting partners. This page on
[portal.vipps.no](https://portal.vipps.no)
shows the accounting partners for one sales unit.

![Overview over accounting-partners](../images/portal-regnskapspartnere-oversikt.png "Accounting Partners overview")

### Adding a new accounting partner

(DRAFT)

This page on
[portal.vipps.no](https://portal.vipps.no)
shows how to add an accounting partners, and how to specify which ledgers the
accounting partner will have access to.

![Add a new accounting-partner](../images/portal-regnskapspartnere-legg-til.png "Add a new accounting partner")
