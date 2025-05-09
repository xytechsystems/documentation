---
title: Methods and Response Codes
weight: 15
---

RESTful services utilize HTTP methods to differentiate between different types of API calls.

## Methods

| Verb   | Used for                                                                                                                                                                                                                                                                                                                                                                    |
| ------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| GET    | - Retrieving a list of all records of a certain document type that match specified criteria, such as all Bids that start in the current year, all Jobs associated with a specific Client, or all Contacts whose names begin with the letter A, or<br>- Retrieving a specific record of a certain document type, such as all the properties of a Job with a specific Job ID. |
| POST   | POST commands are usually used to create new records, such as adding a new resource or creating a new Job                                                                                                                                                                                                                                                                   |
| PATCH  | Updates an existing record with a specified value(s) using partial information<br>Two forms of PATCH are supported:<br>- Specific field replace/delete value (Content-Type : application-json)<br>- Replace using a full JSON payload (Content-Type : application-patch+json) - see note below                                                                              |
| PUT    | Upserts a record (Create or update if already exists using external_key as the identifyer)                                                                                                                                                                                                                                                                                  |
| DELETE | Delete commands are usually used to permanently remove existing records from the host system.   <br>Note: DELETE commands should be used sparingly; to both preserve data integrity and provide historical information, it is usually recommended to change the Status of a record instead of deleting the record completely.                                               |
|        |                                                                                                                                                                                                                                                                                                                                                                             |
 Note: PATCH using 'Content-Type : application-patch+json' cannot be used when calling sub-tables
## Response Codes

The REST service generates HTTP response codes. In many cases, the HTTP response code will be accompanied by additional information in the body or the header of the message. In some cases, the HTTP response code may be the only response.


| Response Code             | Description                                                                                                                                                                                                                                               |
| ------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **2xx Range**             | **Success Codes**                                                                                                                                                                                                                                         |
| 200 Ok                    | The call was successful. In most cases, there will be additional information returned by the API in the body of the message, such as the matching record(s) for a query, or the ID of a record created, modified, or deleted by a corresponding API call. |
| 201 Created               | The request has been fulfilled, resulting in the creation of a new resource.                                                                                                                                                                              |
| 204 No Content            | The server has fulfilled the request but does not need to return a response body. The server may return the updated meta information.                                                                                                                     |
| **4xx Range**             | **Client Error**                                                                                                                                                                                                                                          |
| 400 Bad Request           | The call was not successful due to an error in the URL or the syntax of the API call. Check the syntax of the API call to make sure there are no invalid characters.                                                                                      |
| 401 Unauthorized          | The call was not successful because the API requires a valid<br>credentials, and they were not provided as part of the call.<br>Response Code Description Verify that the call is providing login information in an acceptable manner.                    |
| 404 Not Found             | The call was not successful because the service could not find the requested API. Check the URL for any mismatches between the API call being sent and the documented API signature.                                                                      |
| **5xx Range**             | **Server Error**                                                                                                                                                                                                                                          |
| 500 Internal Server Error | The call was not successful because it caused an error in the service during processing, such as providing an incorrect data type for a given property/field. Verify that all parameters are correct, and values are valid.                               |
| 502 Bad Gateway           | The call was not successful because the service got an invalid response from the API. Verify that all parameters are correct, and values are valid.                                                                                                       |

For more information refer to the [appropriate section of the HTTP protocol](https://www.rfc-editor.org/rfc/rfc9110.html) or a developer resource for HTTP status codes, such as the [MDN Web Docs.](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)
