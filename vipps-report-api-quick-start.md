---
title: Quick start for the Report API
sidebar_label: Quick start
sidebar_position: 20
description: Quick steps for getting started with the Report API.
pagination_prev: Null
pagination_next: Null
---

import ApiSchema from '@theme/ApiSchema';
import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Quick start

## Before you begin

Be aware that these are running on the production server, <https://api.vipps.no>.

🔥 **Don't store production keys in the cloud version of Postman, because this introduces a risk of exposure.** 🔥

The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.

## Getting your ledgers

### Step 1 - Setup

You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal.

You will need the following values, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started):

* `client_id` - Client_id for a test sales unit.
* `client_secret` - Client_id for a test sales unit.
* `Ocp-Apim-Subscription-Key` - Subscription key for a test sales unit.
* `merchantSerialNumber` - The unique ID for a test sales unit.

### Step 2 - Authentication

For all the following, you will need an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`](https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost).
This provides you with access to the API.

```bash
curl https://api.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: YOUR-MSN" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X POST \
--data ''
```

The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

### Step 3 - Get all ledgers

Send
[`GET:/settlement/v1/ledgers`](https://developer.vippsmobilepay.com/api/report#/paths/~1settlement~1v1~1ledgers/get)
to get the ledgers you have access to.

```bash
curl https://api.vipps.no/settlement/v1/ledgers \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

### Step 4 - Get all ledgers

Send
[`GET:/report/v1/ledgertransactions`](https://developer.vippsmobilepay.com/api/report#/paths/~1report~1v1~1ledgertransactions/get)
for a list of payments/transactions.

Set `ledgerDate` to a value in format YYYY-MM-DD `2022-10-01`.
Set `ledgerId` to a 6-digit value (e.g., `302321`).

```bash
curl https://api.vipps.no/report/v1/ledgertransactions?ledgerDate={ledgerDate}&ledgerId={ledgerId} \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

## Next Steps

See the [Report API guide](./api-guide/README.md) to read about the concepts and details.
