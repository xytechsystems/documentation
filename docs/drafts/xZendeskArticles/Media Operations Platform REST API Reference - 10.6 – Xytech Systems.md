## **Introduction**

This document describes the Media Operations Platform REST API, which is the interface by which a third-party component can make REST calls over HTTP to the Platform application server endpoint and receive responses from it. It does not cover any of the following topics:

-   Web Hooks – please refer to the following article called [Webhooks user guide 10.4.](https://helpcenter.xytechsystems.com/hc/en-us/articles/20338332222107) 
-   The Platform SOAP API
-   Platform Service Order Adapters
-   Other outbound interfaces, such as alternate authentication systems.  
    Although most of this document is relevant to the v1 API, it also includes features that only exist in the v2 API and are primarily intended for users of the v2 API.

## Summary of what's new in v10.6

-   Swagger documentation now includes the [PATCH method using a POST payload](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF36JJ5WR17BWBNHKZZCCQR1).
-   Swagger includes example payloads for sub-tables (previously not populated).
-   Swagger now includes [PATCH method for list documents.](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF40NRXV8NZWCRJ5PV9TMSZ1)
-   Other Swagger improvements and correct missing parameter fields.
-   Additional header [parameters for optimization and filtering](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HKSPJS614F0QQG81TQQDCZ24)
    -   [Ignore alternate keys](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HKSQ234JHJYHVREZWD7C867W)
    -   [Omit null values](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HKSQ339VZTSRBSD25DCPTR8A)
    -   [Filter results for maintenance documents](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF434Z8JY77TBZKP9TZXEN8A)

## **Definitions**

<table><tbody><tr><td><span><strong>Platform</strong></span></td><td><span><strong>Media Operations Platform application&nbsp;</strong></span></td></tr><tr><td>API</td><td>Application Program Interface is a set of functions or procedures offered by a host system that allows one or more applications, services, or systems to send information to, request information from, or make changes to the host system.</td></tr><tr><td>REST</td><td>Representational State Transfer is an architectural style of principles that define a set of constraints for how two systems can communicate over a network such as the World Wide Web. The REST architecture is described in <a href="https://helpcenter.xytechsystems.com/hc/en-us/articles/20339398168219#h_01HF387Q6QAWSQ2EZMVZPAYQ1J">REST Basics</a></td></tr><tr><td>Verb</td><td>In REST, this is the programmatic method used to initiate an API command, usually GET, PUT, POST, or DELETE. See also <a href="https://helpcenter.xytechsystems.com/hc/en-us/articles/20339398168219#h_01HF42TTJMW7WJRZ5CQ9GZMFSB">HTTP Methods</a> for more information.</td></tr><tr><td><br>Swagger</td><td>A third-party tool that generates API documentation automatically based on the Open API specification. It presents a browsable, usable Web site as a URL within the API Web service. See also Open API.</td></tr><tr><td>cURL</td><td>cURL is a mechanism for transferring data using networking protocols. It is provided in two ways:<br>• As a command line tool that allows a user to submit requests directly and see the results, usually for testing purposes.<br>• As a library with integrations to several common programming languages, including C++, C#, PHP, and others.</td></tr><tr><td>Model</td><td>A collection of properties, usually defined as name/value pairs or collections of name/value pairs, that make up a document or a portion of a document. An API may accept one or more models as input, to create a new record or modify an existing record using all the specified values. An API may also provide one or more models as output, either as a list of identical models (e.g., retrieving multiple records as part of a list document) or as a collection of related models (e.g., retrieving all models related to a particular Job document).</td></tr><tr><td>Postman</td><td><p>A third-party platform for building and testing APIs; the Postman client application is available on several operating systems and provides a set of tools to test API calls without having to write code and helps debug API calls and response payloads. <a href="https://www.postman.com/">https://www.postman.com/</a></p></td></tr><tr><td>OAS,<br>OpenAPI</td><td>Formerly known as Swagger Specification, the OpenAPI Specification (OAS) defines a standard interface to RESTful APIs. This language-agnostic interface can then be used by code generation, documentation generation, and testing tools to access and understand the underlying API.&nbsp;<br>Refer to <a href="https://swagger.io/specification/" target="_blank" rel="noopener noreferrer">https://swagger.io/specification/</a> for more information.</td></tr></tbody></table>

## **API Basics**

This section describes the fundamental ideas behind APIs, the REST protocol, and the portions of the Platform system involved.

The purpose of an application program interface (API) is to allow an external computer or piece of software (here called an endpoint) to interact with the system that exposes the interface (here called the host), to allow the endpoint to do one or more of the following:

-   Create new records in the host system
-   Retrieve information from the host system
-   Update existing records in the host system
-   Delete existing records in the host system
-   Trigger operations or business logic in the host system

Unlike a user interface, an API is a mechanism for one piece of software to talk to another and usually does not require the action of a person. In some cases, however, an API is used to allow a third-party user interface to interact with the host system.

#### **REST Basics**

Representational State Transfer (REST) is an architectural style of principles that define a set of constraints for how two systems can communicate over a network such as the World Wide Web. These principles are used to create reliable Web APIs where no state information needs to be retained by the host system offering the API.

#### **Features and Benefits**

The REST architecture provides the following benefits:

-   -   Simple – In general, REST calls follow a set of common patterns that can be learned and followed without significant amounts of special syntax.
    -   Stateless - Being “stateless” allows the API provider to handle high volumes of requests with high performance.
        -   The server is not required to keep state information in memory.
        -   The client is not required to connect to a specific server to utilize stored state information, so requests can be distributed among any number of servers.

#### **HTTP Methods**

RESTful services utilize HTTP methods to differentiate between different types of API calls.

<table><tbody><tr><td><span><strong>Verb</strong></span></td><td><span><strong>Communi</strong><strong>ty Used For</strong></span></td></tr><tr><td>GET&nbsp;</td><td>Usually retrieves a representation of the resource’s current state, such as performing a simple query that gathers and returns information about one or more resources (e.g., database records) from the host system.<br>In the Platform API, GET commands are used to retrieve setup documents, maintenance documents, or list documents.<br>Examples include:<br>Retrieving a list of all records of a certain document type, such as all Bids, all Jobs, or all Contacts in the system,<ul><li>Retrieving a list of all records of a certain document type that match specified criteria, such as all Bids that start in the current year, all Jobs associated with a specific Client, or all Contacts whose names begin with the letter A, or</li><li>Retrieving a specific record of a certain document type, such as all the properties of a Job with a specific Job ID.</li></ul>Note: A GET command is entirely contained within the URI sent to the endpoint. As a result, there is a limit on the amount of text that can be sent to the API, so GET commands usually do not support a significant amount of search or filter criteria.</td></tr><tr><td>PATCH</td><td>Patch usually updates an existing state with a specified one using partial information. See also PUT to completely replace an existing state, and POST for updating an existing state in some cases. In the Platform API, usually updates an existing record, such as<br>changing the mailing address of a Contact or changing the status of a Job.</td></tr><tr><td>POST</td><td>Post usually processes the representation provided with the request to create a new representation or update an existing representation.&nbsp;<br>In the Platform API, POST commands are usually used to create new records, such as adding a new resource or creating a new Job.</td></tr><tr><td>PUT</td><td>Put usually updates an existing record by completely replacing an existing state with a specified one or creating a new state where the URI is already known. See also PATCH to replace only a portion of an existing state. In the Platform API, PUT commands will be used to perform upsert calls from v11; refer to POST commands for creating new items or PATCH commands for updating existing items.</td></tr><tr><td>DELETE</td><td>Delete commands are usually used to permanently remove existing records from the host system.&nbsp;<br>Note: DELETE commands should be used sparingly; to both preserve data integrity and provide historical information, it is usually recommended to change the Status of a record instead of deleting the record completely.</td></tr></tbody></table>

**Response Codes**

REST services are built on top of the HTTP protocol, so calls to the REST service generate HTTP response codes. In many cases, the HTTP response code will be accompanied by additional information in the header or body of the message. In some cases, the HTTP response code may be the only response.

<table><tbody><tr><td><span><strong>Response Code&nbsp;</strong></span></td><td><span><strong>Description</strong></span></td></tr><tr><td><strong>2xx range&nbsp;</strong></td><td><strong>Success Codes</strong></td></tr><tr><td>200 Ok</td><td>The call was successful. In most cases, there will be additional information returned by the API in the body of the message, such as the matching record(s) for a query, or the ID of a record created, modified, or deleted by a corresponding API call.</td></tr><tr><td>204 No Content</td><td>The server has fulfilled the request but does not need to return a response body. The server may return the updated meta information.</td></tr><tr><td><strong>4xx range&nbsp;</strong></td><td><strong>Client Error</strong></td></tr><tr><td>400 Bad Request</td><td>The call was not successful due to an error in the URL or the syntax of the API call. Check the syntax of the API call to make sure there are no invalid characters.</td></tr><tr><td>401 Unauthorized</td><td>The call was not successful because the API requires a valid<br>credentials, and they were not provided as part of the call.<br>Response Code Description Verify that the call is providing login information in an acceptable manner.</td></tr><tr><td>404 Not Found</td><td>The call was not successful because the service could not find the requested API. Check the URL for any mismatches between the API call being sent and the documented API signature.</td></tr><tr><td><strong>5xx range&nbsp;</strong></td><td><strong>Server Error</strong></td></tr><tr><td>500 Internal Server Error</td><td>The call was not successful because it caused an error in the service during processing, such as providing an incorrect data type for a given property/field. Verify that all parameters are correct, and values are valid.</td></tr><tr><td>502 Bad Gateway</td><td>The call was not successful because the service got an invalid response from the API. Verify that all parameters are correct, and values are valid.</td></tr></tbody></table>

  
For more information refer to the [appropriate section of the HTTP protoco](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html)l or a developer resource for HTTP status codes, such as the [MDN Web Docs.](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

## **Xytech Platform REST API**

The Platform is, at a fundamental level, a system that deals with creating, updating, and utilizing a particular type of data object referred to as a document.

-   Each Platform document is a representation of a database table or collection of database tables.
    -   Each document has a primary table and may have one or more sub-tables.
    -   All sub-tables are children of the primary table, and a sub-table can have one or more child sub-tables.
-   Each document is one of the following types of documents:
    -   **Setup -** generally describes a single item, and usually only contains a primary table. Setup documents are often used to manage simple items used to generate lists of options in other documents, such as status labels or predefined sets of codes.
    -   **Maintenance** – generally describes either master data (which are used in transactional data) or transactional data. Maintenance documents often contain one or more sub-tables.
    -   **List** documents provide access to sets of other records, such as Setup and Maintenance documents.

The Platform REST API is JSON-based and has Open API v3.0 API specifications for each API call available in YAML. These specifications can be retrieved as a plain YAML file and are also readily available to be viewed in a browser through the Swagger UI. See the Error! Reference source not found. below for more information.

## **High level data model diagram**

This diagram provides you with a high-level understanding of the primary Xytech Platform data objects and how they relate.

Below the name of each data object is the REST API endpoint used for accessing the data object. Where ~/ is prefixed before the endpoint, that indicates it is a sub-table endpoint of the primary endpoint. See the Swagger documentation for full details.

## **!![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20–%20Xytech%20Systems/21936200667675.png)[MediaPulse primary data objects-Summary.drawio.png](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21936200667675.png)**

## **REST API v1 and v2**

#### **Deprecation of v1 API**

-   REST API v1 was the first Xytech REST API product. It had limitations that required breaking changes to improve so v2 was made available from Xytech v9.4 release with improved functionality.
-   REST API v1 will no longer be supported in version 11.0 and beyond.
-   All new integrations should use v2 API and existing integrations should port to v2 as soon as possible
-   To use v2 API, change the base URL version number from ../v1/.. to ../v2/..

#### **Additional Features in v2 API (as of Platform v10.2 release)**

-   **Payload structure is enhanced and updated to be more robust and scalable.**  
    The document name is now always added as the root element of the payload with an array containing the details. 

  
_Example showing the additional root element jm\_job:  
_

<table><tbody><tr><td><strong>API V1 JSON payload for GET JmJob</strong></td><td><p><strong>API V2 JSON payload for GET JmJob</strong></p></td></tr></tbody></table>

<table><tbody><tr><td><p>/JmJob/job_no=11261</p><div><p><img src="Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21892325750043.png" alt="Picture4.png"></p></div></td><td><p>/JmJob/job_no=11261</p><div><p><img src="Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21892325781275.png" alt="Picture5.png" width="305" height="310"></p></div></td></tr></tbody></table>

-   -   See the Swagger documentation for details of the JSON structure for each endpoint.
-   **Additional PATCH capabilities  
    **In addition to the existing v1 PATCH method where each changed field must be defined using the operation/path/value payload structure, you can now use the same JSON payload you would use for a POST call to update a record. This needs to use the header 'Content-Type: application/json-patch+json'. [See the section below for more details.](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF36JJ5WR17BWBNHKZZCCQR1)

-   **Pagination & sorting  
    **API Pagination has been added to the REST API ‘Get’ Query for Lists.  
    **Parameters for:**
    
    <table><tbody><tr><td><p><strong>pageSize&nbsp;</strong></p></td><td>are the number of records returned per page.&nbsp;</td></tr><tr><td><strong>page</strong></td><td>are the page number to return.&nbsp;</td></tr><tr><td><p><strong>sort</strong>&nbsp;</p></td><td>is the field to sort by followed by ascending or descending order.&nbsp;</td></tr></tbody></table>
    
    Example:  
    To return the first 10 records on page 1 sorted by product\_no:  
    GET  
    {server}}/JmOrgProductList?query="active":"Y"}&resultColumns="L":"product\_no","product\_desc"\]}&sort="product\_no\_desc"\]&pageSize=10&page=1
    
    This will allow sites to call for data in manageable payloads without exceeding memory limitations.
    

## **Connecting to the REST API**

#### **Licensing**

While there is a specific license point for the REST API, by default all Platform installations are given access to the API.

The REST API is a component of the Platform’s Application Server. On hosted systems, it is available by default to all customers.

#### **Instance Information You Will Need**

To be able to connect with the REST API, you will need to know the instance:

-   Base URL e.g. ‘https://example.com’ (The same base url you use to access your system)
-   Database name e.g. ‘DEMO1’

For systems installed prior to mid 2023, you may also require the port number. All later systems are now setup with a proxy to avoid the need to know the port number.

If you do not have this information, please contact Xytech Technical Support.

## **Swagger API Documentation**

From Platform version v10.2, a Swagger index page has been introduced to assist in the navigation and creation of Swagger YAML documentation. The Swagger index page can be found by entering the base URL you use for accessing the Platform and adding “/ApiDocs” to the end (e.g., www.xytechexample.com/XYT\_TEST/ApiDocs). The Xytech API Index page is displayed. 

![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21789230401563.png)

**Using the Index**  
The Index is comprised of the following areas:

-   Version: Displayed in the upper left corner of the screen. This shows the API version, currently either v1 or v2.
-   Selected Database: The name of the database currently selected for this host.  
    Only modules for that database are shown.
-   Available Databases: (Where multiple databases exist on the same host) Click a database to select it and view its associated modules.
-   Filter Documents: Enter text into this field and press \[Enter\] to only display matching document descriptions. To clear the filter, delete the text and press \[Enter\].
-   Module and Document List: Only modules in the selected database are displayed. Click any Module to expand and show the documents it contains. Each document has a label that describes its type (Document \[maintenance\], List, or Setup).

#### **For versions prior to v10.2**

For versions prior to v10.2 you will need to generate the Swagger documentation manually for each document endpoint.

Use a browser to generate the Swagger document:

http://{host}:{port}/API/v2/database/{db\_name}/spec/{docName}

For example:

http://myhost:8088/API/v2/database/mp10/spec/JmJob

The above URL will generate the Swagger document and then display the Swagger document.

To display generated Swagger documents, browse to:

http://{host}:{port}/REST/SwaggerUI/dist/index.html?document={docName}\_v2

For example:

http://myhost:8088/REST/SwaggerUI/dist/index.html?document=JmDivision\_v2

## **Authentication**

The REST API currently uses Basic Authentication and will require a database login account.  
Always use HTTPS encrypted protocol when communicating with the REST API to ensure credentials are not passed in clear text.

## **Using Postman as a test client**

This section describes using the third-party Postman application as an API client for testing and troubleshooting purposes. The example uses the JmJob (Job) endpoint.

  
**To execute a GET call to retrieve an existing record:**

1.  Open the Postman application
2.  From File click New and click HTTP Request
3.  Leave the method as GET
4.  Enter the URL:  
    -   http://{base\_url}/API/v2/database/{dbname}/jmJob/job\_no=100 
        -   Replace {base\_url} & {dbname} with your base URL & database name and provide a valid Job Number that exists in your instance.
5.  On the Authorization tab, set Type to Basic Auth and enter a valid username and password, e.g. xytech/xytechpw
6.  Click Send and observe the response.

**To execute a POST call to create a new record:** 

1.  Open the Postman application.
2.  From **File** click **New** and click **HTTP Request**.
3.  Change the method to **POST**.
4.  Add a Header:
    -   Key: **Content-Type**
    -   Value: **application/json**
5.  Enter the URL: http://{base\_url}/API/v2/database/{dbname}/jmJob
6.  On the Authorization tab, set **Type** to Basic Auth and enter a valid username and password, e.g. xytech/xytechpw
7.  On the Body tab, set the type to **json,** and provide a valid JSON object with the minimum fields to create a job. For example, to create a simple Job:

{

    "jm\_job": \[

        {

            "cust\_id": {

                "list\_id": "409"

            },

            "job\_desc": "Passing Fancy",

            "job\_no": {

                "job\_no": \-1

            },

            "job\_type\_no": {

                "job\_type\_no": 11

            }

        }

    \]

}

**NOTES**:

-   -   To generate the primary key job\_no, set the value to -1.  
        When creating multiple sub-records in a single call, subsequent records must advance the primary key ID by -1. E.g -1, -2, -3 etc…
    -   Be aware some fields are mandatory such as Job Type (job\_type\_no) and Customer ID (list\_id).
    -   Numeric values will be accepted with or without quotes.

      8. Click **Send** and observe the response with status code 200 (success with response body).

The id of the created record is returned in a header parameter called 'Location'.

_In v10.6 SP1 a response payload will contain the ID(s) of newly created records._

**To execute a DELETE call to remove an existing record:**

1.  Open the Postman application.
2.  From **File** click **New** and click **HTTP Request**.
3.  Change the method to **DELETE**.
4.  Enter the URL:  
    http://{base\_url}/api/v2/database/{dbname}/JmJob/job\_no=67982
5.  On the Authorization tab, set **Type** to Basic Auth and enter a valid username and password, e.g. xytech/xytechpw
6.  Click **Send** and observe the response with status code 204 (success with no response body).

#### **Using cURL as a test client**

This section describes using the third-party cURL command line utility as an API client for testing and troubleshooting purposes. 

Many REST clients (either via an online tool or desktop application) use a lightweight program called curl to interact over many different types of network protocols (including HTTP, HTTPS, FTP, IMAP, etc.) Windows users may use the curl command from the Command Prompt to call teh REST API and output dumps of useful data.

Curl is included with Windows v10 1803 and later.

_Example curl on Windows to fetch job details and download the response to a file (replace credentails, base url and database name with your values_:

```
curl -u xytech:password <a href="https://{base/">https://{base_</a>url}/api/v2/database/{database name}/JmJob/job_no=342 -H "Accept: application/json" &gt;c:\temp\ApiResponse.txt
```

## **Key fields in the payload structure**

-   The nested sub-group identified by the primary key field name, provides all the key fields of a document. As a minimum, this sub-group always contains the primary key field and an 'external key' field.

_Example shows a section of the Work Order's (jm\_work\_order) key fields in the sub-group wo\_no\_seq._

{

    "jm\_work\_order": \[

      {

        "wo\_no\_seq": {

          "wo\_no\_seq": "2627-1",

          "external\_key": null

        },

        "custom\_field\_data\_no": 63888,

        "wo\_desc": "Dscription2",

        "wo\_desc\_2": null,

...

-   -    In the above example wo\_no\_seq and external\_key are the two key fields that can be used in lookup requests.
    -   Some documents may have additional key fields such as Media Assets (known by the document name lib\_master) See example below.

_Example Media Asset document where there are additional key fields for 'barcode' & 'umid' in the 'master\_no' group._

{

    "lib\_master": \[

        {

            "master\_no": {

                "master\_no": 53,

                "barcode": "1838",

                "external\_key": null,

                "umid": null

            },

            "cust\_id": {

                "list\_id": "23",

                "other\_cust\_id": null,

                "cust\_reference": null,

                "external\_key": null,

                "cust\_id": "23"

            },

            "customer\_name": "Gotta Dance Productions",

            "master\_desc": "A Winter's Tale",

            "barcode": "1838",

            "container\_master\_no": null,

...

-   External key fields exist on all documents and are designed to be used by external systems to store the external system's unique identifier for the record. The external key field can then be used in future look-ups as opposed to the external system having to know the Xytech key field value..

## **Example Postman Collection calls**

An extensive set of example API calls is available via the [Platform’s REST API Postman Collection](https://www.postman.com/xytech-product-team/workspace/xytech-platform-public/collection/25646735-19fb63a8-470d-460d-9eea-d23248239302?action=share&creator=25646735). This collection contains example calls for three types of documents (maintenance, list, and setup). 

We suggest forking the collection to your own workspace which enables you to 'pull' for updates as teh collection grows.

## **PATCH - update datetime fields to 'null'**

It may seem like an obvious approach to use the 'update' operation with a value of 'null' to clear a datetime field, but datetime fields will not get set to null in that way.

The only way to 'null' a datetime field is by using the 'remove' operation.

Example PATCH payload that set's a datetime field to 'null':

\[

    {

        "op": "remove",

        "path": "goodnight\_date",

        "value": null

    }

\]

## **Query parameters**

**GET** and **PATCH** List endpoints support query parameters.

This section describes the syntax and options used for the query parameter. This is a mandatory parameter for List documents.

The general format for a “query” parameter is to add the parameter as _query={}_ to the end of a GET request for a List document after the parameter delimiter (“?”), where the value of query= is a JSON object:

http://{base\_url}/documentList?query={key: value}

Such as:

http://{base\_url}/jmJobList?query={"job\_no": "12345"[}](http://base_url/jmJobList?query={%22job_no%22:%2212345%22})

**Note:** The query parameter is supported only for GET and PATCH requests for List type documents and is **not** supported by GET requests for Setup or Maintenance documents.

In the simplest form, the value is a single piece of information, such as a string or integer. In more complex forms, the value is a JSON object containing specific formats as described below.

In these examples below, the _baseurl_ is assumed to include the document endpoint  
e.g ‘[https://example.com/api/v2/database/example/JmJobList’](https://example.com/api/v2/database/example/JmJobList%E2%80%99)

### TIP: URL encoding of special characters e.g. % and +

When using HTML special characters as part of the query value, they must be URL encoded.

Example:  to use a wildcard query such as "**%dave%"**, the % needs substituting with **%25**.  Once URL encoded will look like this **%25dave%25**

Example GET query with URL encoded wildcard :

{{server}}/MoMediaOrderList?query={"wo\_desc":"**%25dave%25**"}&resultcolumns={"L": \["wo\_no", "wo\_desc"\]}

_(Specifically the reason why **%dave%** fails to return valid results it that **%da** is the encoding for the **Ú** character)_

This also applies to datetime values that use the offset attribute with the + sign.

To include a value of "**2023-06-01T09:00+5:00**" in a URL query parameter, substitute + with **%2b**

Example:

?query={"wo\_begin\_dt":{"$range":\["2023-06-01T09:00**%2b**5:00","2023-06-01T17:00**%2b**5:00"\]}}

-   **String or Number**

<table><tbody><tr><td>Description&nbsp;</td><td>The value in returned items must match a specified string. The string<br>can either be letters or numbers. Wildcard ‘%’ can be used.&nbsp;</td></tr><tr><td>Syntax&nbsp;</td><td>“field”:” value”</td></tr><tr><td><p>Examples:</p></td><td><p><em>baseurl</em>?Query={“job_desc”:” Big Apple Live”}</p><p><em>baseurl</em>?Query={“job_desc”:”%Big%”}</p><p><em>baseurl</em>?Query={“job_no”:101101}</p></td></tr></tbody></table>

To specify multiple key/value pairs, separate each key/value pair with a comma:  
_baseurl_?query={"cust\_id":"123","job\_type\_no":"4"}  
_baseurl_?query={"cust\_id":"123","job\_type\_no":"4","active":"Y"}  
Note: When specifying multiple key/value pairs, the API will return only items that match ALL specified criteria.

-   **Range**

<table><tbody><tr><td>Description</td><td>&nbsp;The value in returned items must fall between a specified minimum<br>and maximum numeric value.</td></tr><tr><td>Syntax&nbsp;</td><td>“field”:{"$range":[lower_limit, upper_limit]}</td></tr><tr><td>Examples</td><td>baseurl?Query={“job_no”:{"$range":[100, 199]}}<br>baseurl?query={"wo_begin_dt":{"$range":["2023-12-01","2023-12-31"]}}<br>baseurl?query={"wo_begin_dt":{"$range":["2023-06-01T09:00","2023-06-<br>01T17:00"]}}</td></tr></tbody></table>

-   **In (Set)**

<table><tbody><tr><td>Description&nbsp;</td><td>The value in returned items must match one of the values provided in a given set of values.</td></tr><tr><td>Syntax&nbsp;</td><td>“field”:{"$in":[“value_1”,”value_2”, … ”value_n”]}</td></tr><tr><td>Examples&nbsp;</td><td>baseurl?Query={“job_no”:{"$in":[100, 105, 110, 119]}}</td></tr></tbody></table>

**TIP: Searching for multiple wildcard values**

If you wanted to search for multiple wildcard values contained within a single field that all need to exist, then you can include a list of wildcard values.

For example, if you wanted to search for media assets that have "Tale" AND "Dark" in their "master\_desc" field, you can use the following query parameter:

{"master\_desc":{$in:\["%Tale%Dark%","%Tale%Dark%"\]}}

Or with encoding of the % symbol (if using Postman) you need:

{"master\_desc":{$in:\["%25Tale%25Dark%25","%25Tale%25Dark%25"\]}}

-   **Null / Empty**

<table><tbody><tr><td>Description&nbsp;</td><td>The value in returned items must be NULL. Note: Put pipe characters<br>around NULL to differentiate it from the literal string “NULL”.</td></tr><tr><td>Syntax&nbsp;</td><td>"field”: “|NULL|”&nbsp;</td></tr><tr><td>Examples&nbsp;</td><td>baseurl?Query={“phone_number”:{"|NULL|"}}</td></tr></tbody></table>

-   **NE (Not Equal)**

<table><tbody><tr><td>Description&nbsp;</td><td>The value in returned items must not match the specified number, string, or NULL.</td></tr><tr><td>Syntax&nbsp;</td><td>“field”:{"$ne":”value”}</td></tr><tr><td><p>Examples:<br>&nbsp;Not equal<br>&nbsp;Not null&nbsp;<br>&nbsp;Not like</p></td><td><p>baseurl?Query={"job_desc ":{"$ne":"Big Apple Live"}}<br>baseurl?Query={"cust_id":{"$ne":1001}}<br>baseurl?Query={"jm_phase_external_key":{"$ne":"|NULL|"}}<br>baseurl?Query={"wo_desc":{"$ne":"Test%"},"wo_type_no": 83}&nbsp;</p></td></tr></tbody></table>

**NULL Values**

The Null parameter returns any record that has a null value for the specified key, which indicates that no value has ever been set. This differentiates it from an empty string for text-based or date-based properties, a 0 value for numbers, and true or false values for Boolean properties.

**Note:** Not all properties support null values. If possible, check whether the database column in the appropriate column allows nulls.

**Keys** are strings and generally match the column name from a given database table. It is  
recommended to match the case of the key name as shown in the Swagger API documentation if possible.

**Values**

\- String values are not case-sensitive.

\- DateTime values should be provided in a valid ISO date format.

## **resultColumns parameter**

Used by the **GET** method on List and (v10.6) Maintenance endpoints.

ResultColumns parameter is used to define the fields you wish to have returned in the response.  
Without this parameter, the response will contain all document fields.

_For example:_  
{baseurl}/JmJobList?Query={"job\_no":2}&resultColumns={"L":\["job\_no","job\_desc"\]}

Job No. and Job Description fields will be included in the response.  
Important to include the “L” top-level element.

**Sub-Table columns**

Many endpoints include related sub-tables in their responses. Example syntax to include specific sub-table columns.

Below example fetches a tranmission order description and all it's service row numbers:

{base\_url}/XmTransmissionOrder/wo\_no\_seq=7655-1?resultColumns={"jm\_work\_order":\["wo\_desc"\],"mo\_service\_row":\["service\_row\_no"\]}

_Response:_

{

    "jm\_work\_order": \[

        {

            "wo\_desc": "WS Transmission Test",

            "mo\_service\_row": \[

                {

                    "service\_row\_no": {

                        "service\_row\_no": 9933,

                        "external\_key": null

                    }

                },

                {

                    "service\_row\_no": {

                        "service\_row\_no": 9934,

                        "external\_key": null

                    }

                }

            \]

        }

    \]

}

**Performance recomendation.** Always use the ResultColumns parameter otherwise responses will return large numbers of fields most of which will not be required and only adds to the system perfromance overhead. In future API versions, this will become a mandatory parameter.

## **Additional parameters**

#### **saveArgument**

In some cases, you must also add a header with the SaveArgument parameter to trigger the app server to perform a function as part of an API call. For example, when creating a Work Order, to load a Work Order Template you must provide the wo\_template\_no value in the payload as well as set the SaveArguement header:

LoadTemplate:  
**SaveArgument : {"LoadTemplate":"2"}**

LoadServiceTemplate, loads a service template to a Media Order or a Transmission Order:

**SaveArgument : {"LoadServiceTemplate":"10"}**

BidApproval, changes the bid approval state of a Bid using an number that maps to the approval

#### **SaveArgument : {"BidApproval":"0"}**

Approval number mapping:

<table><tbody><tr><td>0</td><td><span dir="ltr">Approval</span></td></tr><tr><td>1</td><td><span dir="ltr">Unapproval</span></td></tr><tr><td>2</td><td><span dir="ltr">ApproveAsChangeMemo</span></td></tr><tr><td>3</td><td><span dir="ltr">ApproveAndUnApproveOriginal</span></td></tr><tr><td>4</td><td><span dir="ltr">Abort</span></td></tr></tbody></table>

#### **alternatekeyhandling**

Suppresses additional key fields from the responses. If you do not need to work with key fields other than the primary key, use this parameter to keep the API call performant and reduce the processing overhead when not working with alternate key fields (v10.6).

Applicable to all GET calls for List, Maintenance & Report endpoints.

Values are ‘**ignore**’ or ‘**include**’.

Default value = ‘include’ (includes alternate keys in the response)

Example:

_URL Parameter:- alternatekeyhandling:**include** (default)_ 

{

    "L": \[

        {

            "master\_no": {

                "master\_no": 915,

                "barcode": "MM915",

                "external\_key": "VX-90",

                "umid": null

            },

            "cust\_id": null,

            "company\_name": null,

            "master\_desc": null,

            "barcode": "MM915",

...

_URL Parameter:- alternatekeyhandling:**ignore**_ 

{

    "L": \[

        {

            "master\_no": {

                "master\_no": 915

            },

            "cust\_id": null,

            "company\_name": null,

            "master\_desc": null,

            "barcode": "MM915",

...

Notice how the additional key fields barcode, external\_key & umid are not provided.

It's recommended to always include this header with the value 'ignore', unless working with alternate keys.

#### **nullvaluehandling**

Suppresses all null value fields from the response. Using this parameter reduces the payload size dramatically, especially for larger queries (v10.6).

Applicable to all GET calls for List, Maintenance & Report endpoints.

Values are ‘**ignore**’ or ‘**include**’.

Default value = ‘include’ (includes null values in the response)

Example:

_URL Parameter:- nullvaluehandling=**include** (default)_ 

{

    "L": \[

        {

            "master\_no": {

                "master\_no": 915,

                "barcode": "MM915",

                "external\_key": "VX-90",

                "umid": null

            },

            "cust\_id": null,

            "company\_name": null,

            "master\_desc": null,

            "barcode": "MM915",

...

_URL Parameter:- nullvaluehandling=**ignore**_ 

{

    "L": \[

        {

            "master\_no": {

                "master\_no": 915,

                "barcode": "MM915",

                "external\_key": "VX-90"

            },

            "barcode": "MM915",

...

Notice how all null value fields are omitted.  
It's recommended to always include this parameter with the value 'ignore', unless visibility of null values is required.

#### **Source-Time-Zone-Name**

The REST API uses Date time formats in ISO format with an optional offset value.

e.g. 2014-11-03T22:20:00+00:00

If you omit the offset value when using POST to create a record, you can use a header parameter to set the time zone your dates are using.  
**Key: Source-Time-Zone-Name                 Value: {Windows Time Zone name}**

e.g. : header 'Source-Time-Zone-Name: Pacific Standard Time '

Will create records using Pacific Standard Time as the time zone for the payload times.  
Remember to omit the offset values in your time formats.

## **PATCH Using POST Payload**

There are two PATCH methods, one where you define the field and the operation, the other is to use the full POST JSON payload.

To use the PATCH with the full JSON payload, you must include the header parameter:

**'Content-Type: application/json-patch+json'**

Example Job update (cURL format):

curl --location --request PATCH

'https: //{base\_url}/api/v2/database/{database\_name}/JmJob/job\_no=345' \\

\--header 'Content-Type: application/json-patch+json' \\

\--header 'Authorization: Basic eHl0ZWNoOk1lZGlhMjwqNSE=' \\

\--data '{

  "jm\_job": \[

        {

 "job\_no": {

                "job\_no": 345

            },

            "job\_desc":"Leaves of October",

"job\_type\_no": {

"job\_type\_no": 7

            },

 "po": "PO1234",

 "job\_reference": "Streets Ahead"

        }

    \]

}'

**Notes:**

The payload must include the existing primary key (job\_no in the above example) as a URL parameter as well as in the body payload.

The expected response status code for a successful PATCH is ‘204’ and there will be no response body.

## **PATCH using List endpoints**

List endpoints also support the PATCH method so that you can update multiple records using a query (see ‘GET – Query Parameters’ section above for details on available query parameters). This method is teh equilivant API functionality of Grid Update feature of the UI.  
Example to update multiple jobs using the $in query parameter:

**PATCH** _{base\_url}_/JmJobList?query={"job\_no": {"$in": \["410","411"\]}}

\[

    {

"op": "replace",

"path": "job\_reference",

 "value": "These are not taxed"

    }

\]

As with GET, wildcards ‘%’ are supported such as:

**PATCH** _{base\_url}__/_ JmJobList?Query={"job\_desc": "Sport%"}

## **POST method**

**Mandatory Fields to Create New Items**

The Swagger model identifies many fields as mandatory (nullable: false), which indicates that a value must be provided when the item is written to the database. However, in some cases, the business logic will provide default values, so it may not be strictly necessary to pass values for these fields to the API to create a new record.   
For example, in a basic payload to create a Work Order, Swagger identifies at least 25 fields as mandatory, but the most basic payload to create a Work Order is below:

{

 "jm\_work\_order": \[

        {

 "external\_key": "externalID",

 "wo\_desc": "Match2",

 "wo\_begin\_dt": "2022-01-01T00:00",

"wo\_end\_dt": "2022-01-01T06:00",

   "wo\_type\_no": 2,

"phase\_code": "Bid",

"rate\_card\_no": 1,

   "cust\_id": "6",

"wo\_template\_no": "2"

        }

    \]

}

See other basic payload examples in the public [Postman Collection](https://helpcenter.xytechsystems.com/hc/en-us/articles/21789230412571-Media-Operations-Platform-REST-API-Reference-10-6#h_01HF433TKM2GMHVRE5CPR8X3X6).

## **Custom Field handling**

Any custom fields you have defined through document customization are automatically included in REST API payloads. The naming convention is a combination of the Custom Code you created plus the internal field name concatenated with an underscore. i.e. {customization code}\_{field name}

_Document customization screen showing the Customnization Code_

![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21932257951899.png)

_Document customization screen showing the internal field names:_

![2024-01-10_16-44-24.jpg](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21932249626267.jpeg)

_Response from a GET call to fetch a Work Order showing the payload custom fields:_

![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21932257971867.png)

## **Appendix A – Endpoint List**

There are over 1000 individual documents (endpoints) available to the Platform’s REST API. Below is a small sample of those documents. The full list of available documents can be obtained directly from the Platform using the Document Customizations query found in the System module. Specific documentation on each document can be found via the Swagger site.

<table><tbody><tr><td><strong>ID&nbsp;</strong></td><td><strong>Class Name&nbsp;</strong></td><td><strong>Document Description&nbsp;</strong></td><td><strong>Document Type</strong></td></tr><tr><td>10315&nbsp;</td><td>JmJob&nbsp;</td><td>Job&nbsp;</td><td>Maintenance</td></tr><tr><td>315&nbsp;</td><td>JmJobList&nbsp;</td><td>Jobs&nbsp;</td><td>Select (List)</td></tr><tr><td>10317&nbsp;</td><td>JmJobStatus&nbsp;</td><td>Job Statuses&nbsp;</td><td>Setup</td></tr><tr><td>10318&nbsp;</td><td>JmJobTable1&nbsp;</td><td>Subscription&nbsp;</td><td>Setup</td></tr><tr><td>10322&nbsp;</td><td>JmJobType&nbsp;</td><td>Job Types&nbsp;</td><td>Setup</td></tr><tr><td>359&nbsp;</td><td>JmTrxReport&nbsp;</td><td>Transaction Reports&nbsp;</td><td>Select (List)</td></tr><tr><td>10339&nbsp;</td><td>JmWorkOrder</td><td>Work Order&nbsp;</td><td>Maintenance</td></tr><tr><td>10346&nbsp;</td><td>JmWoTransaction&nbsp;</td><td>Work Order Transactions&nbsp;</td><td>Maintenance</td></tr></tbody></table>

## **Appendix B – Swagger Examples**

![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21789220498843.png)

![](Media%20Operations%20Platform%20REST%20API%20Reference%20-%2010.6%20%E2%80%93%20Xytech%20Systems/21789230406299.png)