<!-- START_METADATA
---
title: API Guide
sidebar_position: 3

---

END_METADATA -->

# Vipps Report API

<!-- START_COMMENT -->

ℹ️ Please use the new documentation:
[Vipps Technical Documentation](https://vippsas.github.io/vipps-developer-docs/).

<!-- END_COMMENT -->

Before you can use this API, you will need to aquire a Authorization token.
This field is named "Authorization" in the request-header and is used to identify your identity and permissions.

See:
[Getting started: Get an access token](https://vippsas.github.io/vipps-developer-docs/docs/vipps-developers/vipps-getting-started#get-an-access-token).

When you have acquired the token, you can request information about your ledgers.
This is done with the `GET:/ledgers` endpoint.

```http
GET https://api.vipps.no/reports/ledgers
Authorization: Bearer W_IfBcSr-askdjhsakjhdasdfgg
```

The response from this will include the ledgers you have access to, some meta-information about the ledger, and a list of relations that describes the accounts that this ledger is connected to.

```json
[
  {
    "ledgerID": "1",
    "settlementAccountNumber": "86011117947",
    "firstPayout": "2022-01-20T03:00:00+02",
    "lastPayout": "2022-01-22T04:00:00+02",
    "org": {
      "orgno": 987654321,
      "jurisdiction": "NO"
    },
    "settlesFor": [
      {
        "type": "epayment",
        "epaymentMSN": "123455"
      },
      {
        "type": "vippsnummer",
        "vippsnummer": "31245",
        "country": "NO"
      }
    ]
  }
]
```

Then, by using the `ledgerID` field from the respose, you can use the `GET:/transactions` endpoint to fetch the transactions for that ledger.
This endpoint can be used to get the data required to generate a settlement report.
It gives you as the consumer a lot of flexibility for how and what you want retrieved, so we recommend looking at the API documentation for all of the options available.

`TODO: Rename to the correct terms and endpoint with parameters.`
![Settlement](./images/adr-settlement-0001.png)

## Questions?

When contacting us about API issues, we are usually able to help faster if you send us
the complete request and response.

We're always happy to help with code or other questions you might have!
Please create an [issue](https://github.com/vippsas/vipps-developers/issues),
a [pull request](https://github.com/vippsas/vipps-developers/pulls),
or [contact us](https://github.com/vippsas/vipps-developers/blob/master/contact.md).

Sign up for our [Technical newsletter for developers](https://github.com/vippsas/vipps-developers/tree/master/newsletters).
