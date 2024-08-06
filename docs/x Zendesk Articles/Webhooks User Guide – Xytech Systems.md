## **Summary**

Introduced with release v10.4 this feature enables users, through the platform UI, to configure triggers that send API payloads to external Web service endpoints. This mechanism allows external systems to initiate workflows based on Xytech-generated messages and make calls back to the platform API to retrieve additional information.

This feature enhances the existing Event Trigger functionality by adding a new notification type. New setup items now exist to define external host URLs and their endpoints. Each trigger specifies the fields used to dynamically generate the JavaScript Object Notation (JSON) payload and the external endpoint to which the payload will be sent. \[10229\]

_Figure 1 - Webhooks Related Data Objects_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/20338456525595.png)

## What's new in release v10.6

-   [Ability to define custom headers for Export Adaptors](https://helpcenter.xytechsystems.com/hc/en-us/articles/20338332222107-Webhooks-User-Guide#h_01HF2WY3AZNSJ1BM3X2E00TCWB)
-   [Enhanced URL fail-over logic for Outbound Connections](https://helpcenter.xytechsystems.com/hc/en-us/articles/20338332222107-Webhooks-User-Guide#h_01HF2X0A0QN1WNGHVRNDT2F0T5)

## **Setup Items**

#### **Outbound Connection**

Outbound Connection defines and sets up the external system’s URL and authentication that are common to one or more endpoints defined by the External Adaptor.

_Figure 2 - Standard Layout of an Outbound Connection_

![|344](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/20338473750299.png)

| Setting | Description |
| --- | --- |
| **Description** | Enter a label for the connection, such as the name and version of the external system or interface. |
| **External Key** | Optionally enter a label to identify the external system, as with any Xytech document. |
| **Authentication Method** | 
Select the method used to log in to the external system:

-   **Basic** - Enter the Username and Password, below.
-   **API Key** – Enter the Key and Value pair into the Username and Password fields, below. Values will be added to the header.
-   **Token** - 

 |
| 

**Base URL 1**

**Base URL 2**

 | 

Enter the base URL(s) of the external system. 

**Base URL 1** is required.

**Base URL 2** is optional. 

See note below on Base URL fail over

 |
| 

**Username**

 | Enter the login account used to authenticate to the external system. |
| 

**Password**

 | Enter the password used to authenticate to the external system. |

**Base URL Fail-over**

Where you have specified a secondary URL, the fail-over logic will be enabled.

If Base URL 1 does not respond after the retry count, set in the Export Adaptor, the system will then fail-over to Base URL 2.

Assuming Base URL 2 is successful, all following triggers will first be sent to Base URL 2.

If Base URL 2 fails to respond, then the system will fail-over back to Base URL 1. 

If both Base URLs are unavailable, the system will give up after the fail-over Base URL fails to respond. (it will not continue to flip-flop between Base URLs)

#### **Export Adaptor**

Export Adaptors define the endpoint(s) for an Outbound Connection. An Export Adaptor is assigned to a specific Event Trigger so that the triggered payload knows where to be sent.

_Figure 3 - Standard Layout for External Adaptor Setup_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21958126786331.png)

| Setting | Description |
| --- | --- |
| **Description** | Enter a label for this adapter, such as the purpose and criteria of the message. |
| **Output Type** | Select the type of system to which messages will be sent. Currently, only REST is supported. |
| **Output Format** | Select the standard used to format outbound data, either JSON or XML. |
| **Response Format** | _Not used_ - (will be used from release v11 for response parsing) |
| **Output Method** | Select the method or verb used to send messages. Available options include POST, PATCH, PUT, and DELETE. |
| **Connection** | Select the Outbound Connection for this Export Adaptor. |
| **Endpoint** | Add the endpoint of the API and include the initial backslash. E.g. /orders. The endpoint can include URL Parameters, as described in _[URL Parameters](https://helpcenter.xytechsystems.com/hc/en-us/articles/20338332222107-Webhooks-User-Guide#url_parameters)_ below If there is no endpoint and the payload can be sent to the base URL, simply add a forward slash ‘/’ into the endpoint field. |
| **Retry Count** | If there is no response from the endpoint, the retry count defines the number of times the system will attempt to send the payload before trying the other Outbound Connection URL (if one has been defined). |
| **Retry Delay** | The duration in seconds between retries when an outbound message does not receive a response. |
| **Time Out** | The duration in seconds to wait for a response from the endpoint. |
| **Disable Logging** | Select (check) to stop capturing activity into the Export Adapter Log. The default setting is deselected (cleared). |
| **Headers** | Provides the ability for the webhook to send custom headers and whether they are to be treated as Content Headers (default = false) |

**URL Parameters  
**URL Parameters can be added to endpoints and include a combination of static text and dynamically populated values from the triggered document. Use the same field naming convention as defining the payload template for an Event Trigger.  
_Examples:_

-   -   {base URL}/{endpoint}/external\_key=\[DOC.external\_key\]
        
    -   {base URL}/{endpoint} /job\_no=\[DOC.job\_no\]/jm\_episode
        

#### **Event Trigger** 

The Webhooks functionality builds on the existing Event Trigger features of the Media Operations Platform, providing a notification template option where you can assign an Export Adaptor and the payload template.

The Event Trigger defines the criteria that determine when a payload is sent, the method and content of the payload, and the Export Adapter to which it is sent.

-   The Event Triggers and Conditions tabs determine the criteria of when to send payloads.
-   The Notifications tab determines the method used to send the payload.
-   The Notification Template tab determines the destination of the trigger content.

Refer to the _**Online Help**_ for more information about Event Triggers and how to configure them.

_Figure 4 - Event Trigger Source_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21964854619291.png)

| **Event Trigger options** |
| --- |

<table><tbody><tr><td><strong>Active</strong></td><td>Select (check) to make this payload active, or deselect (clear) to stop sending messages for this payload.</td></tr><tr><td><strong>Event Code</strong></td><td>Use the event code of NOTIFY</td></tr><tr><td><strong>Event</strong></td><td><span>Description of the trigger</span></td></tr><tr><td><strong><span>Event Type</span></strong></td><td><p><span>Select when to trigger:</span></p><p><span>INSERT sends messages when a record is first created.</span></p><p><span>UPDATE sends messages when a record is modified.</span></p><p><span>DELETE sends messages when a record is deleted.</span></p></td></tr><tr><td><strong><span>Document</span></strong></td><td><span>Select the data object for the trigger.</span></td></tr><tr><td><strong><span>Doc Table Name</span></strong></td><td><span>Select the sub-table of the document for the trigger.</span></td></tr><tr><td><strong><span>Access</span></strong></td><td><span>Select Global to trigger payloads for the entire system, or select an access method (Division or Work Group) to limit payloads to a specific subset of the system.</span></td></tr><tr><td><strong><span>Division</span></strong></td><td><span>When Access is set to Division, select the Division for which payloads will be sent. Only documents related to the selected Division will generate payloads.</span></td></tr><tr><td><strong><span>Work Group</span></strong></td><td><span>When Access is set to Work Group, select the Work Group for which payloads will be sent. Only documents related to the selected Work Group will generate payloads.</span></td></tr></tbody></table>

**Conditions** — Defines the field conditions that initiate a trigger, such as when a specific field is set or modified.

 _Figure 5 - Event Trigger Conditions_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21965149659035.png)

**Notifications —** Establishes the notification type – select ‘Email/Alert’ for webhooks.

 _Figure 6 - Event Trigger Notification Type_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21965135300635.png)

**Notification Template** — Defines the action to take for the trigger.

_Figure 7 -Event Trigger Layout for Export Adaptor_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21965149700251.png)

**Export Adaptor**

Select the Export Adaptor to use, as configured in the _[Export Adaptor](https://helpcenter.xytechsystems.com/hc/en-us/articles/20338332222107-Webhooks-User-Guide#export_adapter)_ section above.

**Generate Default Template**

Click to generate a standard payload template for the selected Document, Event Type, and Export Adaptor.

If any of the Document, Event Type, or Export Adaptor Type definitions change, you should re-generate the default template which will overwrite the previous template. If manual changes have been made to the template, you can update manually or note the additions prior to regenerating the template and reapply the changes.

**Export Adaptor Template**

-   Displays the payload template used to generate the payload.
-   Either in JSON or XML format. (The format of the template should match the format specified in the Export Adapter that will use the template.)
-   Caution needs to be taken to ensure correct syntax is used and avoid adding carriage returns that add invisible or illegal characters.
-   It’s recommended to construct the template in a syntax-checking text editor outside of the Xytech UI first before pasting it back into the UI to avoid errors.
-   If there are errors, this will be visible in the export adaptor log during trigger sending and will fail to send.
-   Users can add document fields to the payload using the syntax \[DOC.fieldName\]  
    Optionally, users can drag and drop a field from the Template Fields list on the left into the payload template field. Note that you can only use fields from the triggered document and not related tables.

_Figure 8 - Dragging a field from the Template Fields list into the payload template._

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21958631095195.png)

**_Prefix File Naming Conventions_**

-   The generic ‘DOC’ prefix before the field name is an abbreviation/placeholder for the triggered document. E.g. DOC.wo\_no\_seq
-   You can also use the table name as the prefix to the field name.  
    E.g. jm\_work\_order.wo\_no\_seq

## **Export Adaptor Logs**

When Export Adaptors are enabled for logging, all external communications will be logged into the Export Adaptor Log to assist with auditing and troubleshooting. (By default, logging is enabled for all adaptors.) From the navigation menu, click **System** and click **Export Adaptor Logs** to view the log entries.

_Figure 9 - Export Adaptor Logs List_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/20338627703195.png)

Click the ID in the **Export Adaptor Log No** column to view the details of a log entry, including all relevant values such as date times and request/response payloads.

_Figure 10 - Export Adaptor Log Entry_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/20338643730331.png)

The Action Menu in individual documents also displays the Export Adaptor Logs for that specific item. For example, when viewing a Work Order, the Action filters the log entries to that specific Work Order only.

 _Figure 11 - Export Adaptor Logs on the Work Order Action Menu_

![](Webhooks%20User%20Guide%20%E2%80%93%20Xytech%20Systems/21958631099419.png)

#### **Auto Log Clean-Up**

To prevent the log tables from growing exponentially due to large payload values, a periodic clean-up process removes the request and response payloads from "old" log items. The system retains the log entry that provides information that the payload was sent but removes the payloads. The default setting for this process is to clean up log items older than 60 days. This setting can be manually overwritten if required via the Application Server configuration settings.

## **Webhook Errors Notification**

If you want to be notified by email or an alert pop-up when a webhook fails to send or receives an error response, you can set up an additional Event Trigger on the Export Adaptor Log with a condition set to trigger on receiving an error response status code (or whatever is the most appropriate trigger condition for your integration).

## **Troubleshooting**

**Trigger Condition Report**

While testing an Event Trigger, it’s recommended to validate the trigger conditions by using the Event Trigger Report run from the modified record to check whether the trigger condition is met prior to saving the record.

**Test by Creating a Mock Web Service using Postman**

There are many ways to create a mock Web service for testing. One of the easiest is to use Postman.