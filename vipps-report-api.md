<!-- START_METADATA
---
title: "API Guide: Overview"
sidebar_position: 30
---
END_METADATA -->

# Vipps Report API: Overview

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

## Table of contents

* [About the Report API](#about-the-report-api)
* [Authenticating to the Report API](#authenticating-to-the-report-api)
* [Questions?](#questions)

<!-- END_COMMENT -->

## About the Report API

The Report API serves data about your payments with Vipps, in particular
aggregate information across APIs, and reports containing data for many
payments at once.

This document contains common traits of the entire API.
We also have guides for specific use cases:

* [Settlement guide](vipps-report-api-settlement-guide.md) explains the settlement process and the ledger
* Coming later: Endpoints for recent/non-settled transactions.

**Please note:** The information fetched from the Report API is
asynchronous and trailing behind the other APIs. Usually it is behind
by a second or so, but if we have operational problems it could worst
case be behind by several hours. Therefore, you should always use other
Vipps APIs as the source of truth for the status of an operation.

## Authenticating to the Report API

Currently, access to the Report API is only provided through
using the merchant's API key that grant access to a single MSN.
See
[Which API keys give access to the API?](vipps-report-api-faq.md#which-api-keys-give-access-to-the-api)
for details about these.

We are currently working on providing partners access to the API,
and such support will be launched in the future.

<!--
There are currently three ways to connect to the Report API:

* Using the partner's API keys, called
  [partner keys](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/partner-keys).
* Using the Accounting partner's API keys.


See:
[Getting started: Get an access token](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/vipps-getting-started#get-an-access-token).

### Using the merchant's API keys

Individual merchants that have API keys
for using the eCom/ePayment APIs may use this to access a single
ledger connected to the sale unit (identified with MSN). If there
is more than one sale unit connected to the ledger, access will be denied.

### Using the partner's partner keys

Partner API users do not have access to any ledgers by default. Such
access must be granted by the merchant:
[Adding a new accounting partner](#adding-a-new-accounting-partner).


## Give access to an accounting partner

Merchants must give access to their accounting partner on
[portal.vipps.no](https://portal.vipps.no).

### Overview of accounting partners

A merchant may have zero or more accounting partners. This page on
[portal.vipps.no](https://portal.vipps.no)
shows the accounting partners for one sale unit.

![Overview over accounting-partners](./images/portal-regnskapspartnere-oversikt.png "Accounting Partners overview")

### Adding a new accounting partner

This page on
[portal.vipps.no](https://portal.vipps.no)
shows how to add an accounting partners, and how to specify which ledgers the
accounting partner will have access to.

![Add a new accounting-partner](./images/portal-regnskapspartnere-legg-til.png "Add a new accounting partner")

-->

## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-report-api/issues),
a [pull request](https://github.com/vippsas/vipps-report-api/pulls),
or [contact us](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/contact).

Sign up for our [Technical newsletter for developers](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/newsletters).
