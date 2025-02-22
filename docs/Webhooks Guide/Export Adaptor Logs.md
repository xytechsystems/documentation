---
title: Export Adaptor Logs
weight: 6
---

When Export Adaptors are enabled for logging, all Export Adaptor communications will be logged into the Export Adaptor Log to assist with auditing and troubleshooting. (By default, logging is enabled for all adaptors.) From the navigation menu, click **System** and click **Export Adaptor Logs** to view the log entries.

_Figure 9 - Export Adaptor Logs List_

![](assets/Pasted%20image%2020240807141340.png)

Click the ID in the **Export Adaptor Log No** column to view the details of a log entry, including all relevant values such as date times and request/response payloads.

_Figure 10 - Export Adaptor Log Entry_

![](assets/Pasted%20image%2020240807141351.png)

## Action Menu 
There is an Action Menu item on individual documents to display a filtered Export Adaptor Log for that specific document. For example, when viewing a Work Order, the Action Menu item filters the log entries to that specific Work Order only.

 *Export Adaptor Logs on the Work Order Action Menu*

![](assets/Pasted%20image%2020240807141403.png)

## Auto Log Clean-Up

To prevent the log tables from growing exponentially due to large payload values, a periodic clean-up process removes the request and response payloads from "old" log items. The system retains the log entry that provides information that the payload was sent but removes the payloads. The default setting for this process is to clean up log items older than 60 days. This setting can be manually overwritten if required via the Application Server configuration settings.
