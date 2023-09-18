<!-- START_METADATA
---
title: Report API changelog
sidebar_label: Changelog
sidebar_position: 200
pagination_next: null
pagination_prev: null
---
END_METADATA -->

# Changelog

All notable changes to the current API will be documented in this file.
To learn about API versioning, see
[Common topics: API Lifecycle](https://developer.vippsmobilepay.com/docs/vipps-developers/common-topics/api-lifecycle/).

## Version 2 compared to version 1

* The information in the `/ledgertransactions` endpoint has been split into
  different endpoints to list changes to balance (`funds`) and per-transaction
  fees charged by Vipps MobilePay AS (`fees`). This split is primarily done to make it more natural
  in the API to provide fee specifications also for merchants that receive gross settlements.
* Instead of query parameters to `/ledgertransactions` implying different modes
  of using the endpoint, we provide different endpoints for different ways
  of fetching/synchronizing the data.
* In total `GET:/ledgertransactions` is replaced by the following:
  * `GET:/ledgers/<ledgerId>/funds/feed` 
  * `GET:/ledgers/<ledgerId>/funds/dates/<ledgerDate>`
  * `GET:/ledgers/<ledgerId>/fees/feed`
  * `GET:/ledgers/<ledgerId>/fees/dates/<ledgerDate>`
* While all the above endpoint are very similar to the old `/ledgertransactions` endpoint, there are some cosmetic:
  * `transactionId` has been renamed `pspReference` to be consistent with the [ePayment API](https://developer.vippsmobilepay.com/api/epayment/).
  * `orderId` has been renamed `reference` to be consistent with the [ePayment API](https://developer.vippsmobilepay.com/api/epayment/).
  * `ledgerAmount` is simply `amount`
  * `transactionType` has been renamed to `entryType`
    * The `payout` type has been renamed to `scheduled-for-payout`.
      Please consult the full list of entry types in the reference.
  * The `grossAmount` and `fee` columns are removed from this endpoint and replaced with:
    * Detailed information about fees available on the `/fees` endpoint; both for cases where
      fees are retained ("net settlements") and not ("gross settlements")
    * For net settlements, an adjustment of the ledger balance is included as a sum row;
      typically once such row per day although details of this will vary according to when
      Vipps MobilePay legally collects the fees (`entryType` of `fees-retained`).
  * The `?inPayout` feature has been removed; we recommend to instead fetch data per
    date.
    * We *may* add the feature back if there is enough popular demand for it,
      in that case as a separate `payouts` path alongside `feed`
      and `dates`.
  * The `?sincePayout` has been removed and appears to be unused


## History

* September 2023: Launched final non-Beta version of the Report API, which is named `report/v2`. Version 1 is deprecated.
* November 2022: Launched Beta of Report API, `report/v1`.
