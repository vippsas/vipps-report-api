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

This document covers the quick steps for getting started with the Report API.
You must have already signed up as an organization with Vipps MobilePay and have
your test credentials from the merchant portal, as described in the
[Getting started guide](https://developer.vippsmobilepay.com/docs/getting-started).

**Important:** The examples use standard example values that you must change to
use *your* values. This includes API keys, HTTP headers, reference, etc.

## Getting your ledgers

Be aware that these are running on the production server, <https://api.vipps.no>.

### Step 1 - Setup

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

**Please note:** To prevent your sensitive data and credentials from being synced to the Postman cloud,
store them in the *Current Value* fields of your Postman environment.

In Postman, import the following files:

* [Vipps Report API Postman collection](/tools/vipps-report-api-postman-collection.json)
* [Vipps API Global Postman environment](https://github.com/vippsas/vipps-developers/blob/master/tools/vipps-api-global-postman-environment.json)

Update the *Current Value* field in your Postman environment with your own values (see
[API keys](https://developer.vippsmobilepay.com/docs/common-topics/api-keys/)):

* `client_id` - Merchant key required for getting the access token.
* `client_secret` - Merchant key required for getting the access token.
* `Ocp-Apim-Subscription-Key` - Merchant subscription key.
* `merchantSerialNumber` - Merchant ID.
* `base_url_production` - Set to: `https://api.vipps.no`.

</TabItem>
<TabItem value="curl">

No setup needed :)

</TabItem>
</Tabs>

### Step 2 - Authentication

For all the following, you will need an `access_token` from the
[Access token API](https://developer.vippsmobilepay.com/docs/APIs/access-token-api):
[`POST:/accesstoken/get`](https://developer.vippsmobilepay.com/api/access-token#tag/Authorization-Service/operation/fetchAuthorizationTokenUsingPost).
This provides you with access to the API.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get Access Token
```

</TabItem>
<TabItem value="curl">

```bash
curl https://api.vipps.no/accessToken/get \
-H "client_id: YOUR-CLIENT-ID" \
-H "client_secret: YOUR-CLIENT-SECRET" \
-H "Ocp-Apim-Subscription-Key: YOUR-SUBSCRIPTION-KEY" \
-H "Merchant-Serial-Number: 123456" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X POST \
--data ''
```

</TabItem>
</Tabs>

The property `access_token` should be used for all other API requests in the `Authorization` header as the Bearer token.

### Step 3 - Get all ledgers

Send
[`GET:/settlement/v1/ledgers`](https://developer.vippsmobilepay.com/api/report#/paths/~1settlement~1v1~1ledgers/get)
to get the ledgers you have access to.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get ledgers
```

</TabItem>
<TabItem value="curl">

```bash
curl https://api.vipps.no/settlement/v1/ledgers \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1Ni <truncated>" \
-H "Ocp-Apim-Subscription-Key: 0f14ebcab0ec4b29ae0cb90d91b4a84a" \
-H "Vipps-System-Name: acme" \
-H "Vipps-System-Version: 3.1.2" \
-X GET
```

</TabItem>
</Tabs>

### Step 4 - Get all ledgers

Send
[`GET:/report/v1/ledgertransactions`](https://developer.vippsmobilepay.com/api/report#/paths/~1report~1v1~1ledgertransactions/get)
for a list of payments/transactions.

<Tabs
defaultValue="curl"
groupId="sdk-choice"
values={[
{label: 'curl', value: 'curl'},
{label: 'Postman', value: 'postman'},
]}>
<TabItem value="postman">

```bash
Send request Get ledger transactions
```

</TabItem>
<TabItem value="curl">

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

</TabItem>
</Tabs>

## Next Steps

See the [Report API guide](./api-guide/README.md) to read about the concepts and details.
