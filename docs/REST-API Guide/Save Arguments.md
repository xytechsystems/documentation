---
title: Save Arguments
weight: 24
---
In some cases, you must also add a header to trigger the app server to perform a function as part of an API call. For example, when creating a Work Order, to load a Work Order Template you must provide the SaveArgument parameter as well as including the wo_template_no value in the payload.

Below is a list of Save Arguments
## LoadTemplate
Loads Work Order Template to an order.
**SaveArgument : {"LoadTemplate":"2"}**
Where the number represents the number of the template to load.

Example when creating an order using a POST call. The template number must be provided in both the header and the payload.

NOTE: currently (up to v11.0, LoadTemplate has a limitation in that it will not load additional child templates assigned to resources. In the webapp UI, the user is prompted whether they wish to include child templates when loading the primary template)
```json
curl --location 'http://{APIbaseURL}/JmWorkOrder' \
--header 'Content-Type: application/json' \
--header 'SaveArgument: {"LoadTemplate":"15"}' \
--header 'Authorization: Basic ••••••' \
--data '{
    "jm_work_order": [
        {
            "wo_desc": "100m Finals",
            "wo_begin_dt": "2024-08-22T10:00:00.000Z",
            "wo_end_dt": "2024-08-22T16:00:00.000Z",
            "wo_type_no": 17,
            "phase_code": "RQST",
            "rate_card_no": 1,
            "cust_id": 27,
            "wo_template_no": 15
        }
    ]
}'
```

## LoadServiceTemplate
Loads a service template to a Media Order or a Transmission Order
**SaveArgument : {"LoadServiceTemplate":"10"}**
Where the number represents the Service Template number to load.

Example using POST method. The service template number must be provided in the header and the payload.
```json
curl --location 'http://{APIbaseURL}/XmTransmissionOrder' \
--header 'Content-Type: application/json' \
--header 'SaveArgument: {"LoadServiceTemplate":"10"}' \
--header 'Authorization: Basic ••••••' \
--data '{
    "jm_work_order": [
        {
            "wo_desc": "Match2010",
            "wo_begin_dt": "2023-11-16T10:00:00.000Z",
            "wo_end_dt": "2023-11-16T16:00:00.000Z",
            "wo_type_no": 17,
            "phase_code": "Bid",
            "rate_card_no": 1,
            "cust_id": 19,
            "service_template_no": 10
        }
    ]
}'
```

## BidApproval
Changes the bid approval state of a Bid using number that represents the approval type.
**SaveArgument : {"BidApproval":"0"}**

Approval number types:
<table><tbody><tr><td>0</td><td><span dir="ltr">Approval</span></td></tr><tr><td>1</td><td><span dir="ltr">Unapproval</span></td></tr><tr><td>2</td><td><span dir="ltr">ApproveAsChangeMemo</span></td></tr><tr><td>3</td><td><span dir="ltr">ApproveAndUnApproveOriginal</span></td></tr><tr><td>4</td><td><span dir="ltr">Abort</span></td></tr></tbody></table>

Example using a PATCH call to set the 'approved' value and the header save argument.
```json
curl --location --request PATCH 'http://{APIbaseURL}/BidVersion/version_no=211' \
--header 'Content-Type: application/json' \
--header 'SaveArgument: {"BidApproval":"0"}' \
--header 'Authorization: Basic ••••••' \
--data '[
    {
        "op": "replace",
        "path": "approved",
        "value": "Y"
    }
]'
```

## VoidWorkOrder
Function to void an Order.
**SaveArgument: {"VoidWorkOrder":"36914-1"}**
Where the number represents the Work Order number sequence.

Example using a PATCH call:
```json
curl --location --request PATCH 'http://{APIbaseURL}/JmWorkOrder/wo_no_seq=2626-1' \
--header 'Content-Type: application/json' \
--header 'SaveArgument: {"VoidWorkOrder":"2626-1"}' \
--header 'Authorization: Basic ••••••' \
--data '[
   {
       "op": "replace",
       "path": "cancel_no",
       "value": 1
   }
]'
```

## UnVoidWorkOrder
Provides the ability to un-void an Order.
**SaveArgument: {"UnVoidWorkOrder":"1067982-1"}**

Example using a PATCH call with the header Content-Type : application/json-patch+json
```JSON
curl --location --request PATCH 'http://{APIbaseURL}/JmWorkOrder/wo_no_seq=1067982-1' \
--header 'SaveArgument: {"UnVoidWorkOrder":"1067982-1"}' \
--header 'Content-Type: application/json-patch+json' \
--header 'Authorization: ••••••' \
--data ''
```

## GroupCode
Function to set the default Group of a scheduling resource.
The Group must already be assigned to the Resource in the Group list.
**SaveArgument: {"GroupCode":"UKPS"}**

Example using a PATCH call with the header Content-Type : application/json-patch+json
```json
curl --location --request PATCH 'http://{APIbaseURL}/SchResource/resource_code=3' \
--header 'Content-Type: application/json-patch+json' \
--header 'SaveArgument: {"GroupCode":"TRX"}' \
--header 'Authorization: Basic ••••••' \
--data ''
```

