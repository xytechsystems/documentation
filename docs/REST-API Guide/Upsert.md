---
title: Upsert / PUT
weight: 25
---
Upsert provides the ability to create records if they don't already exist, or update them if they do. Upsert functionality is provided using the PUT method and is available on all maintenance document types. (v11.0+)

## PUT payload concepts
The most common use of the PUT method will be where a 3rd party identifier is known and you wish to create or update a Xytech record.
The external identifier is likely to be stored in the "external_key" field of the record.

What is important, is to supply the external_key field 
1. In the URL
2. As a payload field within the key field object (this is for lookup of existing record)
3. Within the root body of the payload (this is for inserting the value)
This will ensure correct insert or update functionality.

Example to upsert a Job:
```json
curl --location --request PUT 'https://{APIbaseURL}/JmJob/external_key=PH20250205a' \
--header 'Content-Type: application/json-patch+json' \
--header 'Authorization: Basic ********' \
--data '{
    "jm_job": [
        {
            "job_no": {
                "external_key": "PH20250205a"
            },
            "external_key": "PH20250205a",
            "division_no": 12,
            "cust_id": "010",
            "job_desc": "test2",
            "job_type_no": 163,
            "account_rep_no_1": 18,
            "active": "Y"
        }
    ]
}'
```

You will receive the ID of the created record if one does not already exist matching the external_key.
If you send the same payload again, you will receive a 2xx success code, no new ID as the record already exists (any changes would update the record).


See the OpenAPI definition on endpoint details and the Postman collection for examples.

## Deleting existing sub-table records
When performing an upsert, you may wish to clear any existing sub-table records before adding new ones. This can be done in a single API call by adding a header.

Syntax: `rowAction : {"deleteAll":["{sub-table}]}`
Example: endpoint /LibMaster `rowAction value: {"deleteAll":["lib_master_audio"]}`
This will delete all existing audio records of the library asset (and add new ones if defined in the payload)

Optionally, you can delete multiple sub-tables by including separating comma:
Syntax: `rowAction : {"DeleteAll":["<sub table name>","<sub table name>"]}`

**Full example:**
In this example we're creating or updating a library asset with a barcode identifier, if the record already exists the system will delete any existing audio and title records and create new audio and title records.
```json
curl --location --request PUT 'http://{APIbaseURL}/LibMaster/barcode=XYZ19030504' \
--header 'Content-Type: application/json-patch+json' \
--header 'rowAction: {"DeleteAll":["lib_master_audio","lib_master_title"]}' \
--header 'Authorization: Basic **************' \
--data '{
    "lib_master": [
        {
            "master_no": {
                "barcode": "XYZ19030504"
            },
            "master_desc": "My Content 40",
            "asset_type_no": "1",
            "lib_master_title": [
                {
                    "title_no": {
                        "title_no": "36"
                    }
                }
            ],
            "lib_master_audio": [
                {
                    "audio_desc": "Stereo Left",
                    "audio_content_desc": "English",
                    "track_language": "English",
                    "audio_channel": "1"
                },
                {
                    "audio_desc": "Stereo Right",
                    "audio_content_desc": "English",
                    "track_language": "English",
                    "audio_channel": "2"
                }
            ]
        }
    ]
}'
```







