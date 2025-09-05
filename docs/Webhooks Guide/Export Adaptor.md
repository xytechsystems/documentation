---
title: Export Adaptor
weight: 4
---
Export Adaptors define the endpoint(s) for an Outbound Connection. An Export Adaptor is assigned to a specific Event Trigger so that the triggered payload knows where to be sent.

*Figure 3 - Layout for External Adaptor Setup*

![](assets/Pasted%20image%2020240806174436.png)

| Setting             | Description                                                                                                                                                                                                                                                                                                             |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Description**     | Enter a label for this adapter, such as the purpose and criteria of the message.                                                                                                                                                                                                                                        |
| **Output Type**     | Select the type of system to which messages will be sent. Currently, only REST is supported.                                                                                                                                                                                                                            |
| **Output Format**   | Select the standard used to format outbound data, either JSON or XML.                                                                                                                                                                                                                                                   |
| **Response Format** | Select the expected standard format for the response payload (if a response is expected).                                                                                                                                                                                                                               |
| **Output Method**   | Select the method or verb used to send messages. Available options include POST, PATCH, GET, PUT and DELETE.                                                                                                                                                                                                            |
| **Connection**      | Select the Outbound Connection to use for this Export Adaptor.                                                                                                                                                                                                                                                          |
| **Endpoint**        | Add the endpoint of the API and include the initial backslash. E.g. /orders. The endpoint can include URL Parameters, as described in below If there is no endpoint and the payload can be sent to the base URL, simply add a forward slash ‘/’ into the endpoint field.                                                |
| **Retry Count**     | If there is no response from the endpoint, the retry count defines the number of times the system will attempt to send the payload before trying the other Outbound Connection URL (if one has been defined).                                                                                                           |
| **Retry Delay**     | If there is no response from the endpoint, the retry count defines the number of times the system will attempt to resend the payload. After completing the reties, the Export Adaptor will fail-over to the Outbound Connection URL (if more than one URL has been defined). Zero = no retries after first attempt.     |
| **Time Out**        | The duration in seconds to wait for a response from the endpoint.                                                                                                                                                                                                                                                       |
| **Disable Logging** | Select (check) to stop capturing activity into the Export Adaptor Log.                                                                                                                                                                                                                                                  |
| **Headers**         | Provides the ability for the webhook to send custom headers and whether they are to be treated as Content Headers (default = false).<br>Note: in this release you cannot add a Content-Type override from the system generated one e.g 'application/json' when JSON is the outbound format. This is possible from v11.1 |

## URL Parameters
URL Parameters can be added to endpoints that include a combination of static text and dynamically populated values from the triggered document. Use the same field naming convention when defining the payload template for an Event Trigger.  

_Examples:_
-  Endpoint field: `order/external_key=[jm_job.external_key]`
-  Endpoint field: `order/job_no=[jm_job.job_no]/jm_episode]`

