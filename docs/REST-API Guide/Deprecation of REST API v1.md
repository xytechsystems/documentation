---
title: Deprecation of REST API v1
weight: 3
---

REST API v1 was the first Xytech REST API product. It had limitations that required breaking changes to improve so v2 was made available from Xytech v9.4 with improved functionality.

REST API v1 is no longer supported in version 11.0 and beyond.  

All new integrations should use API v2 and existing integrations should port to v2 as soon as possible.  

To use v2 API, change the base URL version number from ../v1/.. to ../v2/..

## Changes in v2 API 

### Payload structure changes
<p align="justify">The payload structure has been enhanced to be more robust and scalable.
The primary data object name is now always added as the root element of the payload with an array containing the details. 
v2 handles sub tables more gracefully and respects the hierarchy of the tables. With v1, the sub tables were often added to the bottom of the payload at the root level, which made it difficult to determine the relationship between tables and their sub items. With v2, the table names are included in the payload and the sub tables are added as children to maintain the relationships.</p>

*Example showing the root element for JmJob*

| API v1 JSON structure                           | API v2 JSON structure                           |
| ----------------------------------------------- | ----------------------------------------------- |
| ![](assets/Pasted%20image%2020240730142842.png) | ![](assets/Pasted%20image%2020240730142900.png) |




All sub-tables are offset from the root object.
See the OpenAPI documentation for details of the JSON structure for each endpoint.

In addition, v2 has many enhancements as detailed in this document.


## Migrating to API v2 

To assist, below are main steps needed to migrate to API v2:
1. Change your URL to use the v2 path.
2. Adapt to the new and improved payload structure.
3. Utilise new parameters


### 1. Change your URL to the v2 path.

API v1
`http://domain/api/v1/database/XYT_MAIN_V`

API v2
`http://domain/api/v2/database/XYT_MAIN_V`

### 2. Update to handle the improved payload structure.
The main difference is that v2 handles sub tables more gracefully and respects the hierarchy of the tables especially when making POST calls. See the examples below:

#### GET Jm_Job v1:
```json
{
    "cust_id": {
        "cust_id": "4",
        "cust_reference": "a",
        "list_id": "4",
        "other_cust_id": "80000016-1475102220"
    },
    "customer_name": "Xytech Systems (80000016-1475102220)",
    "division_no": {
        "division_code": "BUR",
        "division_desc": "Burbank",
        "division_no": 12
    },
    "jm_episode": [
        {
            "episode_desc": "Come, Now is the Time to Worship",
            "episode_number": "3",
            "job_no": 1
        }
    ],
    "jm_installment": [
        {
            "installment_desc": "First installment",
            "installment_no": 1486,
            "job_no": 1
        }
    ],
    "job_no": {
        "external_key": null,
        "job_no": 1
    },
    "other_cust_id": "80000016-1475102220"
}
```

#### GET Jm_Job v2:
```json
{
	"jm_job": [
		{
			"job_no": {
				"job_no": 1
			},
			"division_no": {
				"division_no": 12
			},
			"cust_id": {
				"cust_id": "4"
			},
			"customer_name": "Xytech Systems (80000016-1475102220)",
			"title": "Hungary",
			"job_type_no": {
				"job_type_no": 1158
			},
			"jm_episode": [
				{
					"job_no": 1,
					"episode_det_no": {
						"episode_det_no": 1
					},
					"episode_number": "3",
					"episode_desc": "Come, Now is the Time to Worship",
				}
			],
			"jm_installment": [
				{
					"installment_no": {
						"installment_no": 1486
					},
					"installment_desc": "First installment",
					"term_no": {
						"term_no": 33
					},
					"invoice_type": {
						"invoice_type": 1
					},
				},
			]
		}
	]
}
```

#### POST XmTransmissionOrder v1:
```json
{
    "wo_desc": " AS XM Test 7",
    "wo_begin_dt": "2024-09-07T00:00:00-07:00",
    "wo_end_dt": "2024-09-07T00:00:00-07:00",
    "wo_type_no": {
        "wo_type_no": 3301
    },
    "phase_code": {
        "phase_code": "Conf"
    },
    "cust_id": {
        "list_id": "403"
    },
    "xm_mode_type": "O",
    "xm_mode_no": {
        "xm_mode_no": 12
    }
}
```

#### POST XmTransmissionOrder v2:
```json
{
    "jm_work_order": [
        {
            "wo_desc": " AS XM Test 7",
            "wo_begin_dt": "2024-09-07T00:00:00-07:00",
            "wo_end_dt": "2024-09-07T00:00:00-07:00",
            "wo_type_no": {
                "wo_type_no": 3301
            },
            "phase_code": {
                "phase_code": "Conf"
            },
            "cust_id": {
                "list_id": "403"
            },
            "xm_mode_type": "O",
            "xm_mode_no": {
                "xm_mode_no": 12
            },
            "mo_service_row": [
                {
                    "service_row_no": {
                        "service_row_no": -1
                    },
                    "service_header": "Y",
                    "service_no": {
                        "service_no": 482
                    },
                    "service_profile_no": {
                        "service_profile_no": 1264
                    },
                    "service_status_no": {
                        "service_status_no": 3,
                        "service_status_code": "RDY"
                    },
                    "service_profile_desc": "Media Broadcast BNS"
                },
                {
                    "service_row_no": {
                        "service_row_no": -2
                    },
                    "service_header": "N",
                    "service_no": {
                        "service_no": 482
                    },
                    "service_profile_no": {
                        "service_profile_no": 1264
                    },
                    "service_status_no": {
                        "service_status_no": 3,
                        "service_status_code": "RDY"
                    },
                    "parent_service_row_no": -1,
                    "service_profile_desc": "Media Broadcast BNS",
                    "mo_operation": [
                        {
                            "operation_no": {
                                "operation_no": -1
                            },
                            "task_no": {
                                "task_no": 200,
                                "task_code": "ALW"
                            },
                            "service_row_no": -1,
                            "status_no": 2,
                            "task_code": "ALW",
                            "trx_group_code": {
                                "group_code": "0004S",
                                "trx_group_code": "0004S"
                            },
                            "trx_resource_code": {
                                "resource_code": "FCR10",
                                "trx_resource_code": "FCR10"
                            },
                            "trx_phase_code": {
                                "phase_code": "Conf",
                                "trx_phase_code": "Conf"
                            },
                            "trx_sched_qty": 1,
                            "lock_billing_unit": "N"
                        }
                    ]
                }
            ]
        }
    ]
}
```

### 3. Utilise new parameters
To improve responsiveness and design efficient integrations.
The following additional URL parameters were added for GET requests:
#### NullValueHandling=ignore
This parameter removes all the fields with null values from the result set.
#### AlternateKeyHandling=ignore
This parameter instructs the API to not resolve the alternate keys in the payload for GET requests. The highlighted items are removed from the result set:
![](assets/Pasted%20image%2020240807225619.png)

Best practice:
#### ResultColumns
Always filter a GET call using resultColumns parameter so that you only return the fields you need.


### 4. Use PUT requests for upserts (create or update)
Avoids making additional calls to check whether a record already exists before creating/updating it.


### 5. Utilise the new response payload with IDs of created records

POST XmTransmissionOrder response payload with record IDs
```json
{
    "message": null,
    "tables": [
        "jm_work_order",
        "mo_service_row",
        "mo_operation"
    ],
    "jm_work_order": {
        "record_identifiers": [
            "wo_no_seq"
        ],
        "records": [
            {
                "wo_no_seq": "1067998-1"
            }
        ]
    },
    "mo_service_row": {
        "record_identifiers": [
            "service_row_no"
        ],
        "records": [
            {
                "service_row_no": 217413
            },
            {
                "service_row_no": 217414
            }
        ]
    },
    "mo_operation": {
        "record_identifiers": [
            "operation_no"
        ],
        "records": [
            {
                "operation_no": 128370
            },
            {
                "operation_no": 128371
            }
        ]
    }
}
```

