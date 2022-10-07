<!-- START_METADATA

title: Vipps settlement concepts
sidebar_position: 5

---

END_METADATA -->

# Vipps Report API

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

<!-- START_TOC -->

# Table of contents

- [About the Report API](#about-the-report-api)
- [Authenticating to the Report API](#authenticating-to-the-report-api)
  - [Using the merchant's API keys](#using-the-merchants-api-keys)
  - [Using the partner's partner keys](#using-the-partners-partner-keys)
- [About report endpoints](#about-report-endpoints)
  - [CSV vs JSON](#csv-vs-json)
- [Field/column documentation](#fieldcolumn-documentation)
- [Give access to an accounting partner](#give-access-to-an-accounting-partner)
  - [Overview of accounting partners](#overview-of-accounting-partners)
  - [Adding a new accounting partner](#adding-a-new-accounting-partner)
- [Questions?](#questions)


<!-- END_TOC -->

Document version: 0.0.5.

## About the Report API

The Report API serves data about your payments with Vipps, in particular
aggregate information across APIs, and reports containing data for many
payments at once.

This document contains common traits of the entire API.
We also have guides for specific usecases:

* [Settlement guide](vipps-report-api-settlement-guide.md) explains the settlement process and the Ledger
* Coming later: Endpoints for recent/non-settled transactions.


**Please note:** The information fetched from the Report API is
asynchronous and trailing behind the other APIs. Usually it is behind
by a second or so, but if we have operational problems it could worst
case be behind by several hours. Therefore, you should always use other
Vipps APIs as the source of truth for the status of an operation.

## Authenticating to the Report API

There are currently two ways to connect to the Report API:
* Using the merchant's own API keys for the sale unit.
* Using the partner's API keys, called
  [partner keys](https://vippsas.github.io/vipps-developer-docs/docs/vipps-partner/#partner-keys).

See:
[Getting started: Get an access token](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/vipps-getting-started#get-an-access-token).

### Using the merchant's API keys

Individual merchants that have API keys
for using the eCom/ePayment APIs may use this to access a single
Ledger connected to the sale unit (identified with MSN). If there
is more than one sale unit connected to the Ledger, access will be denied.

### Using the partner's partner keys

Partner API users do not have access to any ledgers by default. Such
access must be granted by the merchant:
[Adding a new accounting partner](#adding-a-new-accounting-partner).

See:
[Adding a new accounting partner](#adding-a-new-accounting-partner).


## About report endpoints

Some of the endpoints in the report API return a "report"; a list of
payments/transactions. These have a common response format and is
used in the same way.

**Please note:** Depending on the merchant's traffic volume and
selected time intervals, some reports can become quite large
(hundred thousands of rows returned). Currently we only support
downloading a full report in one go. We will not time out a
request server side, so it is OK if the download takes several
minutes and is multiple megabytes. We may provide features
for downloading in *pages* of data at a later point.


### CSV vs JSON

For report endpoints, both CSV and JSON response is available.
The response format is controlled by whether 
the `Accept` header is set to `application/json` or `text/csv`.
It is indicated in the [API Spec](https://vippsas.github.io/vipps-report-api/)
whether the CSV format is available.

For report endpoints a large number of columns (CSV)/fields(JSON)
may be available, and we will keep adding new ones from more sources
of data. Therefore, the caller always pass inn all the columns it wants
to receive back, so that only the data the caller wants is returned.

The CSV and JSON formats always contain the same data. For instance,
if this is the CSV response:
```text
transactionId,reference,price.description
2023432783,payment-123,3% + 0.00
3023423423,payment-124,3% + 0.00
```
then we can know that the JSON will look like this:
```json
{
  "items": [
    {
      "transactionId": "2023432783",
      "reference": "payment-123",
      "price": {
        "description": "3% + 0.00",
      }
    },
    {
      "transactionId": "3023423423",
      "reference": "payment-124",
      "price": {
        "description": "3% + 0.00",
      }
    }
  ]
}
```
The only difference is that in JSON, the fields do not have a given
order and that the distinction between numbers and strings disappear in CSV.
JSON sub-structures become dotted column names in CSV.


## Field/column documentation

This table lists all of the fields (or columns, in CSV) available for
all of the reports. Not all fields apply to all kinds of data, so the
table indicates in which contexts the fields are available.

The fields indicated to be "Numeric ID" are returned as **string** in JSON,
but are positive integers smaller than 2^63.
We use a string because IDs should in general
be passed as strings in JSON, and because some JSON parsers and JavaScript
can only handle numbers as floating point (so, restricted to 2^53). However,
it is safe to parse these and use them as a 64-bit integer key in a database.

TODO: Add the missing documentation.

| Field name        | Type       | Description                                                                     |
|-------------------|------------|---------------------------------------------------------------------------------|
| **Common/**       |            |                                                                                 |
| reference         |            |                                                                                 |
| **ledger/**       |            |                                                                                 |
| transactionId     | Numeric ID |                                                                                 |
| transactionType   | String     | See [transaction types](vipps-report-api-settlement-guide.md#transaction-types) | 
| ledgerDate        |            |                                                                                 |
| ledgerAmount      |            |                                                                                 |      
| grossAmount       |            |                                                                                 |  
| fee               |            |                                                                                 |
| msn               |            |                                                                                 |
| time              |            |                                                                                 |
| price.description |            |                                                                                 |
|                    |           |                                                                                 |


## Give access to an accounting partner

Merchants must give access to their accounting partner on
[portal.vipps.no](https://portal.vipps.no).

### Overview of accounting partners

A merchant may have zero or more accounting partners. This page on
[portal.vipps.no](https://portal.vipps.no)
shows the accounting partners for one sale unit.

![Overview over accounting-partners](./images/portal-regnskapspartnere-oversikt.png "Regnskapspartner oversikt")

### Adding a new accounting partner

This page on
[portal.vipps.no](https://portal.vipps.no)
shows how to add an accounting partners, and how to specify which ledgers the
accounting partner will have access to.

![Add a new accounting-partner](./images/portal-regnskapspartnere-legg-til.png "Regnskapspartner oversikt")


## Questions?

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-ecom-api/issues),
a [pull request](https://github.com/vippsas/vipps-ecom-api/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
