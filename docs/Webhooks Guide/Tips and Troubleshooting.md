---
title: Tips and Troubleshooting
weight: 7
---
## Email Notification of a failure
If you want to be notified by email or an alert pop-up when a webhook fails to send or receives an error response, you can set up an additional Event Trigger on the Export Adaptor Log with a condition set to trigger on receiving an error response status code (or whatever is the most appropriate trigger condition for your integration).

## Troubleshooting

**Trigger Condition Report**
While testing an Event Trigger, it’s recommended to validate the trigger conditions by using the Event Trigger Report run from the modified record to check whether the trigger condition is met prior to saving the record. You can find this on the Action Menu - Document

**Test by Creating a Mock Web Service using Postman**
There are many ways to create a mock Web service for testing. One of the easiest is to use tools such as Postman (https://www.postman.com/) or WireMock (https://wiremock.org/).

## Known Issues
### URL Parameters not populated using GET method (v11.0)
In some scenarios, when configuring an Export Adaptor to include a URL parameter with a value that's a document field, the parameter fails get populated.
A workaround that forces the parameter to be populated correctly, is to additionally add the field to the Alert Template of the Event Trigger. The Alert Template does not need to be configured to actually send the alert (by omitting any alert users).
e.g.
![](assets/Pasted%20image%2020241024105233.png)
This issue is not present from v11.1 onwards.