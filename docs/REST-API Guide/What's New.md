---
title: What's New
weight: 2
---
## <font color="#c00000">v11.3</font>

## OAuth 2.0 OpenID Authentication for REST API
Two methods are supported
- Client Credentials method (system to system access)
- SPA / browser sign-in method (interactive user authentication flow)

Pre-requisites
Your auth provider has been configured and the Xytech app server has been configured. (Azure and Okta/Auth0)
 
Client-Credentials method, from auth provider you needs:
- Access token URL
- Client ID
- Client Secret
- Scope

For SPA method you nee:
- Authorisation URL
- Access token URL
- Client ID
- Client Secret
- Scope

Summary flow:
Obtain token from auth provider
Use token to make REST API calls until token expires.

## JWT Token authentication
For use case scenarios where database user accounts are employed.
Typical use might be for system to system authentication requirements.

Pre-requisite
Xytech app server has been configured for JWT authentication

Summary flow:
Retrieve token passing login credentials in the POST body
Use token for all subsequent API calls, until token expires

See additional authentication user guides for more details.

## Child templates of Work Order Templates are now loaded.
When creating or updating an order you can load a work order template along with any child templates associated. This has been accomplished by adding an additional optional parameter to the existing Save Argument called 'LoadChildTemplates'

e.g. header: 
```
SaveArgument: {"LoadTemplate":"1012","LoadChildTemplates":"Y"}
```
(25746)
## Ability to enable custom drop-down fields to return additional attributes
Users can now enable **custom dropdown fields** (through a new document customisation flag 'Additional API details', so that when called by the REST API will return the additional attributes stored with the dropdown record such as external_key.

Example of a custom drop-down field when 'Additional API details' is <u>not</u> enabled:
![|334](assets/Pasted%20image%2020250820122610.png)
Example of a custom drop-down field when 'Additional API details' <u>is</u> enabled:
![|332](assets/Pasted%20image%2020250820122824.png)
(28200)

## Fixes 
For a full list, refer to the [11.3 release notes](https://helpcenter.fabricdata.com/hc/en-us/articles/35383589106459-Xytech-2025-Release-Release-Log-11-3). 
## Response status codes in Swagger documentation now reflect the correct values for the 2xx range.
Resolves an issue where the OpenAPI (Swagger) documentation for certain REST API endpoints did not accurately reflect the actual HTTP response value, such as the 200 Ok response calling the POST method for the /JmJob endpoint (previously 204).
(26278)
## POST datetime fields ignore offset attribute
Resolves a REST API issue when using POST,  datetime offset attributes on date times values were being ignored. Date formats can include the time offset e.g. "2024-12-05T04:00:00-08:00"
(27932)


## <font color="#c00000">v11.2</font>
Fixes
## JmWoTransaction
Resolved an issue where the API documentation for JmWoTransaction incorrectly omitted the required wo_no_seq parameter.

## API Pagination Count incorrect value for pages after 1
Resolves a REST API pagination issue, where the Pagination Count (found in the response header) was responding with in incorrect value.

## Error Loading Service Templates
Resolves an issue when creating a Media Order or Transmission Order via the REST API where a service row field value was set incorrectly (dsp_seq), creating invalid orders.


## <font color="#c00000">v11.1</font>

## Division
REST API calls now support the ability to pass the Division as an override to the API API user's default Division. 

A new optional header called "Division-Code" exists where you can now include the Division code. This will ensure the correct results are returned for the API users current Division.
The API user account must have user access to the division to be able to successfully pass it in the API call. If not, you will receive an error message. 

e.g.
Division-Code : GS
In Postman:
![](assets/Pasted%20image%2020250116145717.png)

## Response body on creation
POST calls now include sub-table IDs of any sub-table records created by the call.

## Attachment File Upload
The REST API now provides the ability to upload an attachment file that auto creates the attachment record. 

The file type needs to already exist as a defined Attachment Type (System - Setup - Attachment Types) to be successfully uploaded. The ability to delete is also provided.

The attachment record will be auto created assigning the matching Attachment Type and description as the filename.  

Multiple files can be uploaded in a single POST API call.
Use **form-data** for the body.

Endpoint:
`/SysAttachment/attachment_keystring=10339:100-1`
Where *attachment_keystring* is made up of a concatenation of document ID and record ID

e.g. 
- Document ID for Work Orders = 10339 (see document customisation list for IDs)
- A specific Work Order ID = 100-1
The response will include the IDs of the created attachment records.

Full cURL example:
```BASH
curl --location 'https://devwcumpapp2.xytech.xytechsystems.com/xyt_main/api/v2/database/XYT_MAIN_RUBY_V/SysAttachment/attachment_keystring=10339:100-1' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic xxxxxxxxxxxxxxxxxxxx' \
--form '=@"/C:/temp/DetailsOn100-1.txt"' \
--form '=@"/C:/temp/A File.pdf"'
```

Postman:
![](assets/Pasted%20image%2020240626105827.png)
Delete attachment file example:
`DELETE {{server}}/SysAttachment/attachment_keystring=10339:100-1/sys_attachment/attachment_no=2205`

## SysContactFacade ability to create User and Vendor profiles
The existing /SysContactFacade endpoint now supports the creation and updating of User and Vendor contact types (profiles).

## Additional query parameters
### NOTIN (Not in)
Ability to query where a field's value does not match an array of supplied values.
Usage example: {{server}}/PmProjectList?query={"project_desc":{"$notin":["test", "Sarah"]}}
Applies to GET queries.
### LIKEAND (Like and)
Ability to define an array of matching values that all have to match regardless of the order defined. 
{{server}}/LibMasterList?{"master_desc":{"$LIKEAND":["%Genesis%","%XHD%"]}
Applies to GET queries.

## Swagger/OpenAPI definition now downloadable as JSON
The Swagger document definition now provides the ability to download the definition as a JSON file instead of a YAML.

## Link child titles to parent in single API call
REST API now provides a new endpoint that gives the ability to manage new or existing Titles and assign the Title to a parent Title in a single API call. New endpoint: /LibTitleHierarchyFacade.

## Known issues
### There are differences in the way Swagger reports a payload definition between POST & GET calls.

Swagger defines the payload for a sub-table GET differently to how the REST API actually responds.


## <font color="#c00000">v11.0</font>  
## 'upsert' is now supported via the PUT method
Ability to create or update if record already exists. see: [Upsert](Upsert.md)

## Compression is now supported
Compression is enabled by default when making calls using a standard header:  
`Accept-Encoding:  gzip or deflate`  
This applies to all API methods.

A couple of real-world examples that shows the benefit of using compression, where a list of transactions were retrieved using a GET list endpoint for a date range:

Example #1  
*Without compression:*  Takes 5 seconds and has a payload of **4.4 MB**.  
*With compression:*  Takes 4 seconds and has a payload of **142 KB**.  

Example #2  
*Without compression:*  22 seconds with response size of **9.47 MB**  
*With compression:*  8 seconds with response size of **320 KB**  
In this example, compression made the payload a 30th the size.

It is recommend to always use compression.

## Greater than and less than new query parameters
You can now use query parameters with:
- LessThanOrEqual ($lte)
- LessThan ($lt)
- GreaterThanOrEqual ($gte)
- GreaterThan ($gt)

e.g. Title numbers greater than 106438
{"title_no":{"$gt": "106438"}}


## Ability to use resultColumns for defining sub-table fields to return on maintenance docs.
When calling a maintenance document using a GET that includes a URL sub-table filter, you can now use the 'resultColumns'  parameters to define the fields you want returned in the response body. Previously the resultColumns parameter was only respected for the primary table and all fields were returned for the sub-table. By specifying the fields you want returned helps maintain overall system performance.

Example defining the field value you want returned from a Job subtable called Episode:
```
{{server}}/JmJob/job_no=342/jm_episode?resultColumns={"jm_episode":"title_no"}
```
This will return only respond with the Episode title_no values for job 342

A more complex example where there is a sub-sub-table are involved:
```
{{server}}/XmTransmissionOrder/wo_no_seq=1072992-1/mo_service_row/service_row_no=221083/mo_operation/operation_no=133119?resultColumns={"mo_operation":["operation_no","task_no","trx_resource_desc","trx_group_desc"]}
```
Returns Operation details for a transmission order for a specific Service Row.

## Corrected null date values  
Fixed in v10.6SP1 and v11.0
Creating date fields with a null value now correctly sets the date value to null as opposed to "0001-01-01T00:00:00"
Applies to POST/PUT/PATCH methods.

## SysContactFacade
POST to SysContactFacade no longer responds with the ID of the created record in the header, instead it now responds with the ID within the response body. This is due to the fact that SysContactFacade is capable of creating multiple new records and the header can only provide a single ID value.

## <font color="#c00000">v10.6</font>

-   Swagger documentation now includes the PATCH method using a POST payload
-   Swagger includes example payloads for sub-tables (previously not populated).
-   Swagger now includes PATCH method for list documents.
-   Other Swagger improvements and correct missing parameter fields.
-   Additional header parameters for optimization and filtering
    -   Ignore alternate keys
    -   Omit null values
    -   Filter results for maintenance documents