---
title: Examples
weight: 16
---
Some basic examples of API calls
## GET
Retrieve Work Order header details returning specific fields and not null values:
```json
curl --location --globoff 'http://{APIbaseURL}/JmWorkOrder/wo_no_seq=2627-1?resultColumns={"jm_work_order":["wo_no_seq","wo_desc","wo_begin_dt","wo_end_dt","phase_desc","sched_by_name"]}&nullvaluehandling=ignore' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic *************' \
--data '@'
```

Response:
```json
{
    "jm_work_order": [
        {
            "wo_no_seq": {
                "wo_no_seq": "2627-1"
            },
            "wo_desc": "Dscription2",
            "wo_begin_dt": "2000-03-31T02:30:00-08:00",
            "wo_end_dt": "2000-03-31T05:30:00-08:00",
            "wo_type_no": {
                "wo_type_no": 3275,
                "wo_type_desc": "Production"
            }
        }
    ]
}
```

## POST method
Create a new Work Order with minimal fields:
```json
curl --location 'http://{APIbaseURL}/JmWorkOrder' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic ********' \
--data '{
    "jm_work_order": [
        {
            "wo_desc": "Match20",
            "wo_begin_dt": "2023-08-22T10:00:00.000Z",
            "wo_end_dt": "2023-08-22T16:00:00.000Z",
            "wo_type_no": 17,
            "phase_code": "Bid",
            "rate_card_no": 1,
            "cust_id": 19
        }
    ]
}'
```

Response:
```json
{
    "message": null,
    "tables": [
        "jm_work_order"
    ],
    "jm_work_order": {
        "record_identifiers": [
            "wo_no_seq"
        ],
        "records": [
            {
                "wo_no_seq": "8250-1"
            }
        ]
    }
}
```
## PATCH

```json
curl --location --request PATCH 'http://{APIbaseURL}/JmJob/job_no=67981' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic eHl0ZWNoOnh5dGVjaHB3' \
--data '[
    {
        "op": "replace",
        "path": "job_desc",
        "value": "Hello World"
    },
    {
        "op": "replace",
        "path": "external_key",
        "value": 15411
    }
]'
```

Response: Status Code 204


## PATCH Using POST Payload

There are two PATCH methods, one where you define the field and the operation, the other is to use the full POST JSON payload.
To use the PATCH with the full JSON payload, you must include the header parameter: **'Content-Type: application/json-patch+json'**

Example Work Order update using PATCH and the POST payload structure:
```json
curl --location --request PATCH 'http://{APIbaseURL}/JmWorkOrder/wo_no_seq=8248-1' \
--header 'Content-Type: application/json-patch+json' \
--header 'Authorization: Basic **********' \
--data '{
    "jm_work_order": [
        {
            "wo_no_seq": {
                "wo_no_seq": "8248-1"
            },
            "wo_desc": "My New Description",
            "po": "123456",
            "phase_code": "Hold"
        }
    ]
}'
```
Response 200 status code
```json
{
    "message": null,
    "tables": []
}
```

   
**Notes:**
The payload must include the existing primary key as a URL parameter as well as in the body payload.
The expected response status code for a successful PATCH can be 200 or ‘204’ where no response is returned.

## PATCH using List endpoints

List endpoints also support the PATCH method so that you can update multiple records using a query (see ‘Parameters’ section above for details on available query parameters). This method is the equilivent API functionality of the Grid Update feature in the UI.  

Example to update multiple Work Orders using the $range query parameter:

```json
curl --location --globoff --request PATCH 'http://{APIbaseURL}/JmWorkOrderList?Query={"date_added":{"$range":["2022-07-26T00:00:00","2023-07-28T00:00:00"]},"wo_type_no": 40}' \
--header 'Content-Type: application/json' \
--header 'Authorization: ••••••' \
--data '[
  {
    "op": "replace",
    "path": "wo_reference",
    "value": "Quick Service 2"
  }
]'
```
Response Status Code 204.

As with GET, wildcards ‘%’ are supported such as:
**PATCH** _{base\_url}__/_ JmJobList?Query={"job\_desc": "Sport%"}


See many other basic payload examples in the public [Postman Collection](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF433TKM2GMHVRE5CPR8X3X6).
