---
title: Errors
sidebar_position: 37
pagination_prev: Null
pagination_next: Null
---

# Errors

Here are the Report API Errors with description.
See the
[API guide](overview.md)
for all the details.

For more common questions, see:

* [General FAQ](https://developer.vippsmobilepay.com/docs/faqs)

## Error-codes

### Code: 10001

Unknown or undocumented error. This is a catch-all error that we in time want to reduce the frequency of as much possible.
The reason of the error might be because of an ephemeral issue on our end, and you should be able to retry the same query again.

### Code: 10002

Input validation error. One of your inputs did not get parsed properly or is missing.
The message in the response should give you a better description about which field is required.
