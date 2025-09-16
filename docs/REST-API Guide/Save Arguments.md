---
title: Save Arguments
weight: 24
---
In some cases, you must also add a header to trigger the app server to perform a function as part of an API call. For example, when creating a Work Order, to load a Work Order Template you must provide the SaveArgument parameter as well as including the wo_template_no value in the payload.
#### Multiple Save Arguments
Where applicable, multiple save arguments can be used as an array separated by a comma. Example:
```
SaveArgument : {"LoadServiceTemplate":"10","11"},{"LoadTemplate":"2"}
```


Below is a list of Save Arguments

## Load a Template to an order
### Applicable endpoints: JmWorkOrder, MoMediaOrder, XmTransmissionOrder
Loads Work Order Template to an order.
```
SaveArgument : {"LoadTemplate":"2"}
```
Where the number represents the number of the template to load.

To load child templates (available from release 11.3), add an additional parameter called 'LoadChildTemplates'
```
SaveArgument: {"LoadTemplate":"1012","LoadChildTemplates":"Y"}
```

Example when creating an order using a POST call. The template number must be provided in both the header and the payload.

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

Multiple Service Templates can be loaded by including comma separated values:

```
SaveArgument : {"LoadServiceTemplate":"10","11"}
```

In this scenario you can leave the "service_template_no" in the body as null. 

## Approve a Bid
### Applicable endpoints: BidVersion
Changes the bid approval state of a Bid using number that represents the approval type.

```
SaveArgument : {"BidApproval":"0"}
```

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

## Void a Work Order
### Applicable endpoints: JmWorkOrder, MoMediaOrder, XmTransmissionOrder
Function to void an Order.

```
SaveArgument: {"VoidWorkOrder":"36914-1"}
```

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

## Un-Void a Work Order
### Applicable endpoints: JmWorkOrder, MoMediaOrder, XmTransmissionOrder
Provides the ability to un-void an Order.

```
SaveArgument : {"UnVoidWorkOrder":"1067982-1"}
```

Example using a PATCH call with the header Content-Type : application/json-patch+json
```JSON
curl --location --request PATCH 'http://{APIbaseURL}/JmWorkOrder/wo_no_seq=1067982-1' \
--header 'SaveArgument: {"UnVoidWorkOrder":"1067982-1"}' \
--header 'Content-Type: application/json-patch+json' \
--header 'Authorization: ••••••' \
--data ''
```

## Set the default Group of a Scheduling Resource
### Applicable endpoints: SchResource
Function to set the default Group of a scheduling resource.
The Group must already be assigned to the Resource in the Group list.

```
SaveArgument : {"GroupCode":"UKPS"}
```

Example using a PATCH call with the header Content-Type : application/json-patch+json
```json
curl --location --request PATCH 'http://{APIbaseURL}/SchResource/resource_code=3' \
--header 'Content-Type: application/json-patch+json' \
--header 'SaveArgument: {"GroupCode":"TRX"}' \
--header 'Authorization: Basic ••••••' \
--data ''
```

## Actualise Work Order actuals
### Applicable endpoints: JmActual
v11.1
To actualize selected or all transactions.
This save argument will update the transaction and order phase, effectively posting the actuals.

```
saveArgument : { "ActualizeSelected":"38521,38522", "ActualizeUpdatePhase":"Y"}
```

Variations:

```
saveArgument : { "ActualizeAll":"-2", "ActualizeUpdatePhase":"Y"}

SaveArgument : { "ActualizeSelected":"-2,200003,200004", "ActuallizeUpdatePhase":"Y"}
```

Intended to be used in conjunction with the POST of actual actions. This save argument then performs the action of actualization.

Example single transaction actualization without times:
```json
curl --location 'http://{APIbaseURL}/JmActual/' \
--header 'Content-Type: application/json' \
--header 'SaveArgument: { "ActualizeSelected":"1259520", "ActualizeUpdatePhase":"Y"}' \
--header 'Authorization: ••••••' \
--data '{
    "jm_actual_header": [
        {
            "actual_hdr_no": {
                "actual_hdr_no": -1
            },
            "wo_no": 9453,
            "wo_seq": 1,
            "wo_no_seq": {
                "wo_no_seq": "1075881-1"
            },
            "actual_status": "Y",
            "jm_actual_detail": [
                {
                    "actual_hdr_no": -1,
                    "actual_det_no": -2,
                    "action_code": {
                        "action_code": "I"
                    },
                    "actual_unit": 4,
                    "actual": "Y"
                }
            ],
            "jm_actual_link": [
                {
                    "actual_link_no": -3,
                    "actual_det_no": -2,
                    "trx_no": 1259520
                }
            ]
        }
    ]
}'
```

## Post Time Card Batch
### Applicable endpoint: TcBatch
v11.1
Replicates the action UI menu item to 'Post' the batch. 
```
saveArgument : {"Post":""}
```

Example PATCH payload:
```JSON
curl --location --request PATCH '/TcBatch/batch_no=7572' \
--header 'Content-Type: application/json-patch+json' \
--header 'saveArgument: {"Post":""}' \
--header 'Authorization: Basic aHR0cDovL2RldndjdW1wYXBwMjoxMTAwMC9hcGkvdjIvZGF0YWJhc2UvWFlUX01BSU5fVjp4eXRlY2hwdw==' \
--data '{
    "tc_batch": [
        {
            "batch_no": {
                "batch_no": "7572"
            }
        }
    ]
}'
```


