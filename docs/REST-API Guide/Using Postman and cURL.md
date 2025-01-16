---
title: Using Postman and cURL
weight: 17
---
## Xytech Public Postman Collection
An extensive set of example API calls is available via the [Platform’s REST API Postman Collection](https://www.postman.com/xytech-product-team/workspace/xytech-platform-public/collection/25646735-19fb63a8-470d-460d-9eea-d23248239302?action=share&creator=25646735) which you can your fork so that you can pull for future updates. This collection contains many example calls, including basic payload version to get you started.

## Using cURL as a test client
cURL is a lightweight command line API client used for testing and troubleshooting.

Example curl to fetch job details (replace authorization string, baseURL and database name with your values)
```http
curl --location 'https://baseURL/api/v2/database/DEMO/JmJob/job_no=342' \
--header 'Content-Type: application/json' \
--header 'Authorization: Basic eHl0ZWNoOnh5dGjaHB3' \
```
