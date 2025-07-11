---
title: Key Fields and Time Formats
weight: 21
---
Each payload contains a nested sub-section of key fields which are used for the lookup of records by a key field. This sub-subsection is identified by the primary key field name. 

The nested sub-section of fields are not used for writing values to the database, values that need to be stored should be contained in the main root of the payload.

The sub-group always contains the primary key field and the 'external_key' field.

Additionally, there are sub-sections of related tables with their key fields. Lookups (using GET query parameters) can only filter sub-tables using the sub-table primary key and not their alternate keys, unless sub-table fields exist in the main body of the payload.

As a general rule, if you are creating or updating file values, the fields you need to write values to should be included in the root of the payload


## Creating Records with key fields
Using POST, you have two options for creating and setting key values of sub-tables.
Below is an example to create a JmJob that generates the primary key value (using -1) and assigns the Division, Customer and Job Type sub-table IDs.
This structure follows the same structure you will receive from a GET call.
```json
{
    "jm_job": [
        {
            "job_no": {
                "job_no": -1
            },
            "division_no": {
                "division_no": 2
            },
            "cust_id": {
                "cust_id": 409
            },
            "job_desc": "Passing Fancy",
            "external_key": "1234",
            "po": "PO002",
            "job_type_no": {
                "job_type_no": 1113
            }
        }
    ]
}
```

Alternatively you can flatten the structure to populate the primary key values. This is where the field name is the same as the sub-section name.
```json
{
    "jm_job": [
        {
            "job_no": -1,
            "division_no": 2,
            "cust_id": "409",
            "job_desc": "Passing Fancy",
            "external_key": "1234",
            "po": "PO002",
            "job_type_no": 1113
        }
    ]
}
```

## Creating records with alternate keys
When using POST to create new records, values for any alternate key fields, such as external_key, must be contained in the main body of the payload and not included in the sub-section of key fields (where they will be ignored). The same applies to PATCH method that uses the header `application/json-patch+json` 
See examples above where external_key is included in the POST payload.
Or this example for creating a resource with an external key value:
```json
{
    "sch_resource": [
        {
            "resource_code": {
                "resource_code": "FRED01"
            },
            "resource_desc": "Fred",
            "external_key": "Ext001"
        }
    ]
}
```

## External Key
External key fields exist on all documents and is designed to be used to store the external system's unique identifier for a record. The external key field can then be used in future look-ups as opposed to having to know the Xytech primary key field value.

Example below shows a GET response section of the Job's (jm_job) key fields in the sub-group "job_no":
```JSON
{
    "jm_job": [
        {
            "job_no": {
                "job_no": 342,
                "external_key": "154110"
            },
            "division_no": {
                "division_no": 2,
                "division_desc": "Burbank ",
                "division_code": "BRB",
                "external_key": null
            },
            "cust_id": {
                "list_id": "409",
                "other_cust_id": null,
                "cust_reference": null,
                "external_key": null,
                "cust_id": "409"
            },
            "other_cust_id": null,
            "external_key": "154110",
            "customer_name": "Tupper Lake Films",
            ...
```

 Some documents may have additional key fields which will all be contained within the same sub-group, such as Media Assets (known by the document name lib_master). All these key fields within "master_no" sub section can be used to retrieve the record with a GET call.

```json
{
    "lib_master": [
        {
            "master_no": {
                "master_no": 18,
                "barcode": "2000XYT",
                "external_key": null,
                "umid": null
            },
            "cust_id": {
                "list_id": "32",
                "other_cust_id": null,
                "cust_reference": null,
                "external_key": null,
                "cust_id": "32"
            },
            "customer_name": "Alchemy Films",
            "master_desc": "International Master  
            ...
```

GET call example to retrieve the above record using barcode:
```json
{{server}}/LibMaster/barcode=2000XYT
```

NOTE: You will need to ensure uniqueness is maintained across alternate keys if you want to be able to retrieve individual records like the above example.

## Generating the primary key 
When creating records using the POST method, to generate the primary key, set the value to -1.  
When creating multiple sub-table records in a single call that need their own next value generated, advance the value by one for each separate sub-table key field needed, E.g -1, -2, -3 etc…
If you use -1 again, it will use the same value as the first instance. This can be useful in calls that require the same ID value to be populated again elsewhere in the payload.

There is one exception to this which is **JmWorkOrder**, where the primary key is a combination of wo_no + wo_seq. To generate wo_no_seq requires a "+1" value.
Example:
```json
{
    "jm_work_order": [
        {
            "wo_no_seq": "+1",
            "wo_desc": "Match20",
            "wo_begin_dt": "2023-08-22T10:00:00.000Z",
            "wo_end_dt": "2023-08-22T16:00:00.000Z",
            "wo_type_no": 17,
            "phase_code": "Bid",
            "rate_card_no": 1,
            "cust_id": 19
        }
    ]
}
```

Be aware some fields are mandatory, and are defined in the OpenAPI definition.

The id of the created record is returned in the response payload.

## Time formats

Date time formats used by the REST API follow ISO-8601 standards. `YYYY-MM-DDThh:mm:ss.sTZD`

Times are stored in the Xytech database as UTC times 
(the exception being when the Master Time Zone is not set to UTC, which is a legacy option)

The recommendation is to work in UTC times or local times using the Time Zone Header attribute.

### POST, PUT and PATCH Examples

UTC time format:
2025-02-20T10:00:00.000Z
2025-02-20T10:00:00Z

Or
Using time zone offset:
2025-02-16T04:00:00-05:00

Or
use the header 'Source-Time-Zone-Name' to define the time zone.
e.g.  header: `Source-Time-Zone-Name: Pacific Standard Time` 

Use this if you want to supply API payload times in local times, any DST offsets will be auto calculated when the system stores the UTC time into the database.

This will override any time zone offset supplied.

### GET and response payloads
Will respond with the offset attribute.
2025-02-20T10:00:00+00:00

(Note any offset is determined by the app server time zone , normally set to UTC, the time plus offset will equate to the UTC time)


