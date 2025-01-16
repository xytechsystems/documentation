---
title: OpenAPI Definition and Endpoints
weight: 19
---
## Summary Index
From Platform version v10.2, an OpenAPI (previously referred to as Swagger) index page has been introduced to assist in the navigation and generation of OpenAPI definitions. The index page can be found by entering the base URL of your site (you use for accessing the Platform UI) and adding “/ApiDocs” to the end (e.g. `www.xytechexample.com/XYT\_TEST/ApiDocs`). The Xytech API Index page is displayed. 

*API Index Page*
![](assets/Pasted%20image%2020240730145018.png)

## Using the Index
The index page displays groups of endpoints called modules for the selected database. Clicking on a module will reveal the endpoints within that module. Click on an endpoint, and the OpenAPI definition will be generated using the Swagger UI.

The Index is comprised of the following areas:
-   Selected Database: The name of the database currently selected for this host. 
-   Available Databases: (Where multiple databases exist on the same host) Click a database to select it and view its associated endpoint modules.
-   Filter Documents: Enter text into this field and press \[Enter\] to display matching endpoint descriptions.
-   Module and Document List: Only modules in the selected database are displayed. Click any Module to expand and show the endpoints it contains. Each endpoint has a label that describes its type (Document \[maintenance\], List, or Setup).


> [!NOTE] Demo OpenAPI definitions
> See a demo of Xytech OpenAPI definitions [here](https://wcudemo0.xytechcloud.com/apidocs)

## Mandatory Fields Note
The OpenAPI definition model identifies many fields as mandatory (nullable: false), which indicates that a value must be provided when the item is written to the database. However, in some cases, the business logic will provide default values, so it may not be strictly necessary to pass values for all mandatory fields to the API to create a new record.   
For example, in a basic payload to create a Work Order, OpenAPI identifies at least 25 fields as mandatory, but the most basic payload to create a Work Order requires 7.

## For versions prior to v10.2
For versions prior to v10.2 you will need to generate the OpenAPI documentation manually for each document endpoint.
Use a browser to generate the OpenAPI document:
`http://{host}:{port}/API/v2/database/{db_name}/spec/{docName}`

For example:
`http://myhost:8088/API/v2/database/mp10/spec/JmJob`

The above URL will generate the OpenAPI document and then display the OpenAPI document.
To display generated OpenAPI documents, browse to:
`http://{host}:{port}/REST/SwaggerUI/dist/index.html?document={docName}_v2`

For example:
`http://myhost:8088/REST/SwaggerUI/dist/index.html?document=JmDivision_v2`

## Endpoint List
There are over 1000 individual documents (endpoints) available to the Platform’s REST API. Below is a small sample of those documents. The full list of available documents can be obtained directly from the Platform UI using the Document Customizations list found under the System module. Specific details on each document can be found via the [OpenAPI definition](OpenAPI%20definition.md) 


<table><tbody><tr><td><strong>ID&nbsp;</strong></td><td><strong>Class Name&nbsp;</strong></td><td><strong>Document Description&nbsp;</strong></td><td><strong>Document Type</strong></td></tr><tr><td>10315&nbsp;</td><td>JmJob&nbsp;</td><td>Job&nbsp;</td><td>Maintenance</td></tr><tr><td>315&nbsp;</td><td>JmJobList&nbsp;</td><td>Jobs&nbsp;</td><td>Select (List)</td></tr><tr><td>10317&nbsp;</td><td>JmJobStatus&nbsp;</td><td>Job Statuses&nbsp;</td><td>Setup</td></tr><tr><td>10318&nbsp;</td><td>JmJobTable1&nbsp;</td><td>Subscription&nbsp;</td><td>Setup</td></tr><tr><td>10322&nbsp;</td><td>JmJobType&nbsp;</td><td>Job Types&nbsp;</td><td>Setup</td></tr><tr><td>359&nbsp;</td><td>JmTrxReport&nbsp;</td><td>Transaction Reports&nbsp;</td><td>Select (List)</td></tr><tr><td>10339&nbsp;</td><td>JmWorkOrder</td><td>Work Order&nbsp;</td><td>Maintenance</td></tr><tr><td>10346&nbsp;</td><td>JmWoTransaction&nbsp;</td><td>Work Order Transactions&nbsp;</td><td>Maintenance</td></tr></tbody></table>
