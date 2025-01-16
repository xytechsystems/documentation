---
title: Webhooks Introduction
weight: 1
---

Introduced with release v10.4 this feature enables users, through the platform UI, to configure triggers that send API payloads to external Web service endpoints. This mechanism allows external systems to initiate workflows based on Xytech-generated trigger events.

![](assets/Pasted%20image%2020240806143454.png)
This feature enhances the existing Event Trigger functionality by adding a new notification type. Setup items:
- Outbound connection - Stores the connectivity details of the external endpoint
- Export Adaptor - Stores one or more endpoints for an Outbound Connection
- Event Trigger - Trigger condition, payload structure and fields used to dynamically generate the JSON or XML payload to send to an Export Adaptor's endpoint.

_Figure 1 - Webhooks Related Data Objects_

![](assets/Pasted%20image%2020240806143105.png)



