---
title: Outbound Connection
weight: 3
---


Outbound Connection defines and sets up the external system’s URL and authentication that are common to one or more endpoints defined by the External Adaptor.

_Figure 2 - Layout of an Outbound Connection_

![](assets/Pasted%20image%2020240806144429.png)

| Setting                   | Description                                                                                                                                                                                                  |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| **Description**           | Enter a label for the connection, such as the name and version of the external system or interface.                                                                                                          |
| **External Key**          | Optionally enter a label to identify the external system, as with any Xytech document.                                                                                                                       |
| **Authentication Method** | Select the method used to log in to the external system:<br>-   **Basic** - Enter the Username and Password, below.<br>-   **API Key** – Enter the Key and Value pair into the Username and Password fields. |
| **Base URL 1**            | **Base URL 1** is required. (exclude any trailing '/')                                                                                                                                                       |
| **Base URL 2**            | **Base URL 2** is optional. (exclude any trailing '/')                                                                                                                                                       |
| **Username**              | Enter the login account used to authenticate to the external system                                                                                                                                          |
| **Password**              | Enter the login account used to authenticate to the external system                                                                                                                                          |

**Base URL Fail-over**

Where you have specified a secondary URL, the fail-over logic will become enabled.

If Base URL 1 does not respond after the retry count, set in the Export Adaptor, the system will then fail-over to Base URL 2.

Assuming Base URL 2 is successful, all following triggers will continue to first be sent to Base URL 2.

If Base URL 2 fails to respond, then the system will fail-over back to Base URL 1. 

If both Base URLs are unavailable, the system will give up after the fail-over Base URL fails to respond. (it will not continue to flip-flop between Base URLs)
