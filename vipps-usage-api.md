<!-- START_METADATA
---
title: API Usage
sidebar_position: 3

---

END_METADATA -->

# API Usage

Before you can use this api, you will need to aquire a Authorization token. This field is named "Authorization" in the request-header and is used to identify your identity and permissions.

`TODO: Text about how to fetch the JWT`

When you have aquired the token, you can request information about your ledgers.
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
