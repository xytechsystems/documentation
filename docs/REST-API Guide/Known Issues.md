---
title: Known Issues
weight: 27
---
## POST & GET payload differences

There are differences in the payload structure between POST & GET calls when filtering by sub-table.
When using GET to fetch a payload filtered by a sub-table, the payload is missing the sub-table wrapper. The Swagger definition reports the correct payload structure that includes the sub-table wrapper, but that is not what works today.
A fix would mean a breaking change, so we shall not be correcting this until API v3.

Example of how Swagger defines the GET response payload for a sub-table of /XmTransmissionOrder endpoint, which includes the wrapper in this case mo_service_row.

```json
{
  "mo_service_row": [
    {
      "service_header": "string",
      "service_desc": "string",
      "service_row_no": {
        "service_row_no": 0,
        "external_key": "string"
      },
      "dsp_seq": 0,
      ...
```

Below is how the the endpoint actually responds that excludes the wrapper.

```json
[
    {
      "service_header": "Y",
      "service_desc": "AUTO ROUTE",
      "service_row_no": {
        "service_row_no": 7428,
        "external_key": null
      },
      "dsp_seq": 1,
      ...
```

## Sub-table record creation respond with invalid IDs
When using a POST call that includes creating sub-table records, some endpoints respond with invalid sub-table record IDs that are negative values. This will be resolved in v11.1

## Field order of some endpoints has to be maintained
There is an issue with a few endpoints when sending a JSON payload. The order the fields  submitted within the payload can cause an error response when placed in an in an order the endpoint does not expect.
Field order should not be a necessity, but to improve this, requires substantial changes to the API that can only be considered for the future API v3.  

## Release v10.6 Date Time format
Temporally in release v10.6, the format of date time values where all expressed in UTC format instead of with the time offset.

Should look like this:

```
"2023-04-21T01:00:00+01:00"
```
For v10.6 the format looked like this:

```
"2023-04-21T00:00:00Z"
```

This has been corrected from v11.0 onwards.

## Release v11.2 Date Time offset ignored for POST
An issue existed in v11.2 where any offset value included with the date time when using POST would be ignored. This has been resolved in v11.3
