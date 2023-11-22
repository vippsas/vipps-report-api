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

![Vipps](./images/vipps.png) *Version 2 is available. Version 1 is deprecated.*

## Before you begin

Be aware that these are running on the production server, [https://api.vipps.no](https://api.vipps.no).

**Please note:*  The Management API is not available in the test environment. See
[Limitations of the test environment](https://developer.vippsmobilepay.com/docs/test-environment/#limitations-of-the-test-environment).

🔥 **To reduce risk of exposure, never store production keys in Postman or any similar tools.** 🔥

The provided example values in this guide must be changed with the values for your sales unit and user.
This applies for API keys, HTTP headers, reference, phone number, etc.

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

**Please note:** This is for v1. V2 is being prepared now.
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

### Step 4 - Get a funds ledger

Send
[`GET:/report/v2/ledgers/{ledgerId}/{topic}/dates/{ledgerDate}`][get-ledgers-endpoint]
for a list of payments/transactions.

Set `topic` to `funds`.
Set `ledgerId` to the 6-digit value that is the unique ledger ID for the ledger you want.
Set `ledgerDate` to a value in format YYYY-MM-DD (e.g., `2022-10-01`).

```bash
curl https://api.vipps.no/report/v2/ledgers/{ledgerId}/funds/dates/2022-10-01 \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

### Step 5 - Get a fees ledger

Send
[`GET:/report/v2/ledgers/{ledgerId}/{topic}/dates/{ledgerDate}`][get-ledgers-endpoint]
for a list of payments/transactions.

Set `topic` to `fees`.
Set `ledgerId` to the 6-digit value that is the unique ledger ID for the ledger you want.
Set `ledgerDate` to a value in format YYYY-MM-DD (e.g., `2022-10-01`).

```bash
curl https://api.vipps.no/report/v2/ledgers/{ledgerId}/fees/dates/2022-10-01 \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

### Step 6 - Get a continuous feed of data (optional)

If you want to continuously stream data as it becomes available, use the
[`GET:/report/v2/ledgers/{ledgerId}/{topic}/feed`][fetch-report-by-feed-endpoint]
endpoint.

Set `topic` to `funds` or `fees`, for the type of ledger you require.
Set `ledgerId` to the 6-digit value that is the unique ledger ID for the ledger you want.


```bash
curl https://api.vipps.no/report/v2/ledgers/{ledgerId}/{topic}/feed \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

## Next Steps

See the [Report API guide](./api-guide/README.md) to read about the concepts and details.



[get-ledgers-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/settlementv1/operation/getLedgers
[fetch-report-by-date-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1dates~1%7BledgerDate%7D/get
[fetch-report-by-feed-endpoint]:https://developer.vippsmobilepay.com/api/report/#tag/reportv2ledgers/paths/~1report~1v2~1ledgers~1%7BledgerId%7D~1%7Btopic%7D~1feed/get
