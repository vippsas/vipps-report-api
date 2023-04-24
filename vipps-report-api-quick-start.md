---
title: Quick start
sidebar_position: 20
pagination_prev: Null
pagination_next: Null
---

# Quick start

Use the Report API to get details for their own sales units and merchants.
In addition, you can order products on behalf of your merchants.

Be aware that these are running on the production server.

## Postman

### Prerequisites

Review
[Vipps quick start guides](https://developer.vippsmobilepay.com/docs/vipps-developers/quick-start-guides)
for information about getting your test environment set up.

### Step 1: Get the Vipps Postman collection and environment

Save the following files to your computer:

* [Vipps Report API Postman collection](tools/vipps-report-api-postman-collection.json)
* [Vipps API Global Postman environment](https://raw.githubusercontent.com/vippsas/vipps-developers/master/tools/vipps-api-global-postman-environment.json)

### Step 2: Import the Vipps Postman files

1. In Postman, click *Import* in the upper-left corner.
1. In the dialog that opens, with *File* selected, click *Upload Files*.
1. Select the two files you have just downloaded and click *Import*.

### Step 3: Set up Postman environment

1. Click the down arrow, next to the "eye" icon in the top-right corner, and select the environment you have imported.
2. Click the "eye" icon and, in the dropdown window, click `Edit` in the top-right corner.
3. Fill in the `Current Value` for the following fields to get started. For the first three keys, go to *Vipps Portal* > *Utvikler* ->  *Test Keys*.
   * `client_id` - Merchant key is required for getting the access token.
   * `client_secret` - Merchant key is required for getting the access token.
   * `Ocp-Apim-Subscription-Key` - Merchant subscription key.
   * `merchantSerialNumber` - Merchant id.
   * `baseUrl` - Set to: `https://api.vipps.no`.

## Make API calls

Be aware that these requests can only be run on the production server.

1. Send request `Get Access Token`. This provides you with access to the API.
   Be sure to use the address to the production server and provide keys for a production sales unit.
   The access token is valid for 24 hours in the production environment.

1. Send request `Get ledgers` to get the ledgers you have access to.
See
[`GET:/settlement/v1/ledgers`](https://developer.vippsmobilepay.com/api/report#/paths/~1settlement~1v1~1ledgers/get),

1. Send request `Get ledger transactions` for a list of payments/transactions. See
[`GET:/report/v1/ledgertransactions`](https://developer.vippsmobilepay.com/api/report#/paths/~1report~1v1~1ledgertransactions?ledgerId=%7BledgerId%7D/get)
