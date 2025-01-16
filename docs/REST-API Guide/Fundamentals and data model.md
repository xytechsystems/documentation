---
title: Fundamentals and data model
weight: 18
---

The Platform is, at a fundamental level, a system that deals with creating, updating, and utilizing a particular type of data object referred to as a document.

-   Each Platform document is a representation of a database table or collection of database tables and has an API endpoint.
    -   Each endpoint has a primary table and may have one or more sub-tables.
    -   All sub-tables are children of the primary table, and a sub-table can have one or more child sub-tables.
-   Each endpoint represents one of the following types of document:
    -   **Setup -** generally describes a single item, and usually only contains a primary table. Setup documents are often used to manage simple items used to generate lists of options in other documents, such as status labels or predefined sets of codes.
    -   **Maintenance** – generally describes either master data (which are used in transactional data) or transactional data. Maintenance documents often contain one or more sub-tables.
    -   **List** documents provide access to sets of other records, such as Setup and Maintenance documents.

The Platform REST API is JSON-based and has Open API v3.0 API specifications for each API call available in YAML (migrating to JSON from v11.1). These specifications can be retrieved as a plain YAML file and are also readily available to be viewed in a browser through the Swagger UI. See the [OpenAPI definition](OpenAPI%20definition.md)

## High level data model diagram

This diagram provides you with a high-level understanding of the primary Xytech Platform data objects and how they relate.

Below the name of each data object is the REST API endpoint used for accessing the data object. Where ~/ is prefixed before the endpoint, that indicates it is a sub-table endpoint of the primary endpoint. See the OpenAPI definition for full details.


![](assets/MediaPulse%20primary%20data%20objects-Summary.drawio.png)