---
title: REST API New in v11.1
---
### v11.1
## Division
REST API GET calls now support the ability to pass the Division as an override to the API user's default Division. A new optional header called "Division-Code" exists where you can now include the Division code. This will ensure the correct results are returned for the API users current Division.
The division must be a Division already assigned to the API user. If not, you will receive an error message. 
e.g.
Division-Code : BUR
[Feature 13684](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/13684)

[Feature 9796](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/9796)

## Response body on creation
Now includes sub-table IDs of created sub-table records.
## Attachment File Upload

The REST API now provides the ability to upload an attachment file that auto creates the attachment record. The file type needs to already exist as a defined Attachment Type to be successfully uploaded. The ability to delete is also provided.

The attachment record will be auto created assigning the matching Attachment Type and description as the filename.  

Multiple files can be uploaded in a single API call.
Use **form-data** for the body.

Endpoint:
`/SysAttachment/attachment_keystring=10339:100-1`
Where *attachment_keystring* is made up of a concatenation of document ID and record ID

e.g. 
- Document ID for Work Orders = 10339 (see document customisation list for IDs)
- A specific Work Order ID = 100-1
The response will include the IDs of eth created attachment records.

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

### Delete attachment file
`{{server}}/SysAttachment/attachment_keystring=10339:100-1/sys_attachment/attachment_no=2205`



## OpenID Authentication method
[19988](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/19988)
Can now obtain bearer token from auth provider and use that token to call the Xytech REST API.
### OpenID / Bearer Token (v11.1)
1. Obtain token from your OpenID auth provider 
Follow the steps provided by your auth provider e.g. Microsoft/Google

2. Use the token to call the REST API
Include token as a header in the format:
`'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1'`  

Note: Tokens will expire, so will need periodic refreshing.

## SysContactFacade ability to create User and Vendor profiles
[16585](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/16585)
The existing SysContactFacade endpoint now supports the creation and updating of User and Vendor contact types (profiles).

## Query parameters
### NOTIN
Usage example: {{server}}/PmProjectList?query={"project_desc":{"$notin":["test", "Sarah"]}}

### LIKEAND
GET query with AND condition, matching on all search keywords where the order of the keywords should not matter.
{"master_desc":{"$LIKEAND":["%Genesis%","%XHD%"]}
[21750](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/21750)

## Swagger Doc - ability to download JSON file definition
[22514](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/22514)
As an API integration developer, I need my integration tools to be able to import Swagger docs as a JSON file. Either downloaded or via a direct URL link. (tools such as Zapier, Node Red OpenAPI node etc...)

This story is to provide users with a JSON file definition to download.

## Link child titles to parent in single API call
REST API now provides a new endpoint that gives the ability to create new or existing Titles and assign to a parent Title in a single API call. New endpoint: /LibTitleHierarchyFacade.


## New Save Arguments
For JmActual


## Known issues
### There are differences in the way Swagger reports a payload definition between POST & GET calls.

Swagger defines the payload for a sub-table GET correctly, but the REST API actually responds incorrectly.

The correct response includes a prefix of the sub-table.
Currently V2 REST API 


Walt:
*There is, however, a problem with the way models (schemas) are generated and used in the Rest API.  There are cases where the payload returned in a get call and the payload expected for a post call are different.  When that happens the problem is actually a bug in the Rest API.  Swagger expects the same model to be used in both of those cases, but they differ because the post expects a model wrapped with the object name (like in your example) but the get returns the model without the object name wrapper.  Unfortunately, making them agree is a breaking change, so this is probably something that shouldn't be addressed until v3 of the API is created.*



