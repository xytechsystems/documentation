---
title: Connecting and Authenticating
weight: 14
---
## Licensing
The REST API is licensed a component of the Platform’s Application Server. 

## Information You Will Need
To be able to connect with the REST API, you will need to know the instance details:  

| Detail        | e.g.                | Comment                                                 |
| ------------- | ------------------- | ------------------------------------------------------- |
| Site Base URL | https://example.com | This is the same base URL you use to access your system |
| Database Name | DEMO1               | Instance database name                                  |

## API URL
The API URL is constructed as follows:

`{Site Base URLl}/api/v2/database/{dbname}/{endpoint}` 

e.g. `https://example.com/mysite/api/v2/database/demo1/JmJob`

For systems installed prior to mid 2023, you may also require the port number. All later systems are now setup with a proxy to avoid the need to know the port number.

If you do not have this information, please contact Xytech Technical Support.

## API Users & Permissions
An API user needs to have been created as a user in the Platform. The user, must be enabled for REST API access by flagging 'Allow API Login' on the user setup. 
As with all users, assign appropriate security roles (enable as a super user is not recommended).

## Authenticating

### OAuth 2.0 OpenID Authentication for REST API
Two methods are supported
- Client Credentials method (system to system access)
- SPA / browser sign-in method (interactive user authentication flow)

Pre-requisites
Your auth provider has been configured and the Xytech app server has been configured. (Azure and Okta/Auth0)
 
Client-Credentials method, from auth provider you needs:
- Access token URL
- Client ID
- Client Secret
- Scope

For SPA method you nee:
- Authorisation URL
- Access token URL
- Client ID
- Client Secret
- Scope

Summary flow:
Obtain token from auth provider
Use token to make REST API calls until token expires.

### JWT Token authentication
For use case scenarios where database user accounts are employed.
Typical use might be for system to system authentication requirements.

Pre-requisite
Xytech app server has been configured for JWT authentication

Summary flow:
Retrieve token passing login credentials in the POST body
Use token for all subsequent API calls, until token expires

See additional authentication user guides for more details.

### Basic Authentication
The REST API uses Basic Authentication and will require a login account.  
Always use HTTPS encrypted protocol when communicating with the REST API to ensure credentials are not passed in clear text.
