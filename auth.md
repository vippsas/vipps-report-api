<!-- START_METADATA
---
title: Setup
sidebar_position: 2
---
END_METADATA -->

# Authenticating to the Report API

There are currently two ways to connect to the Report API. Either way,
the token obtained is passed as a bearer token in the `Authorization` HTTP
header:

```text
GET https://api.vipps.no/report/v1/ledgers
Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni...
```

## Single MSN token

Individual merchants that have an API token
for using the e-com/epayment APIs may use this to access a single
Ledger connected to the MSN.

See https://vippsas.github.io/vipps-developer-docs/api/ecom/#tag/Authorization-Service
for information about how to obtain such a token.

## Partner tokens

TODO: Link to getting partner token

Partner API users do not have access to any ledgers by default. Such
access must be granted by the merchant in the [Vipps portal](https://portal.vipps.no).
See [permissions for accounting partners](grant-access-to-accounting-system.md)
for more information and screenshots for the process.
