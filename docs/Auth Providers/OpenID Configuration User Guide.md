## Introduction
This document explains the steps for setting up and configuring Xytech with an OpenID provider, specifically Microsoft Entra.
The steps detailed in this document relating to Microsoft do not take into consideration your site's own IT policies and standards, but demonstrates the basic requirements needed by Xytech.
This guide includes the server-side settings for on-prem hosted instances.

## Microsoft Entra Admin setup
https://entra.microsoft.com/
### Pre-requisite
- OP User Groups exist that represent the different Xytech user roles. e.g. Scheduler, Operator, Media Manager etc...
- OP Users are defined and assigned to their respective Xytech Group.

OP = OpenID provider
### App Registration
##### Redirect URI
For the App Registration, you will need to know your sites redirect URI.
The redirect URI is the URL you use for logging into Xytech with the endpoint of /openidlogin
For example your Xytech login URL looks like: https://example.com/site/login
The Xytech site redirect URI will be in the form: https://example.com/site/openidlogin

By the end of the App Registration, you will have the following values needed for Xytech configuration:
- Directory (Tennent) ID
- Application (Client) ID
- Secret 
- User Group ID's 

### Create an App Registration
Navigate to *App Registrations* using the Entra UI
- Provide a name e.g. Xytech
- Supported Account Types e.g. Accounts in this organizational directory only
- Web Redirect URI e.g. Your Xytech redirect URI as detailed above.

![](assets/Pasted%20image%2020241018165306.png)
This now gives you the Application ID & Directory ID: 
![](assets/Pasted%20image%2020241018165325.png)

### Secrets
Create a new secret with a description and expiry timeframe.
Make a note of the value of the Secret.

### Token Configuration
Add optional claims - represents the additional information returned in the token.
These are used by the OpenID mapping function in Xytech when creating a new user or mapping to an existing user.

![](assets/Pasted%20image%2020241018165420.png)


### Group IDs
Make a note of the Group Object IDs that are to be mapped as Xytech permitted Groups.

![|400](assets/Pasted%20image%2020241018165433.png)


## Xytech Configuration

### Create Template Users
For each of the user groups defined by the OP, a template user needs to be created or identified.

The group id (Microsoft Group Object ID) from the OP should be added to the Xytech template User's *external_key* field.

Changes made to the template user can be configured to auto sync to the linked OpenID Users, such as changes to Preferences, Divisions, Security, Dashboards and Additional Contacts. This means any changes to template user security roles will automatically be applied to existing OpenID users.

Indicate that the user is a Template user and choose options.

User settings:
![](assets/Pasted%20image%2020241018165459.png)

### Appserver.config settings (on prem instances)

Add the following settings to the Xytech appserver config file:
Replacing {...} with your values obtained earlier for:

- Directory (Tennent) ID
- Application (Client) ID
- Secret 

```cmd
OPENID_AUTH_URL=https://login.microsoftonline.com/{Directory ID}/oauth2/v2.0/authorize
OPENID_TOKEN_URL=https://login.microsoftonline.com/{Directory ID}/oauth2/v2.0/token
OPENID_CLIENT_ID={Client ID}
OPENID_CLIENT_SECRET={Secret}
OPENID_MANAGE_USERS=Y
OPENID_SCOPE=openid offline_access profile https://graph.microsoft.com/.default


SKYWEB_{serverName}_AUTHMETHODS=OPENID,DB
SKYWEB_{serverName}_AUTHUSERCONTROL=Y
```

OPENID_MANAGE_USERS=Y 
- When set to Y and user templates exist in Xytech, user security will be synced with the Xytech template. 
- User has to be assigned a Group in the auth provider.
- Auth provider Group ID must be stored in the external key of the User Template in Xytech
- User Templates in Xytech define the security roles for the auth provider's user group

AUTHMETHODS
Defines the authentication methods a user can select from when logging in.

AUTHUSERCONTROL
Allows users to select from the various authentication methods or forces them to use the first.

#### Changes to auth user
Any changes to claim values in MS Entra user profile or assigned user group are reflected into Xytech on next login. 

### OpenID Mapping
Functionality that obtains the Entra claim values and maps to Xytech database fields.
(v11.1 Use 'sub' as the unique ID in order to be compatible for system to system auth via API configuration)

| Type          | Description   | Claim         | Path        | Maps To | Column Name  | Resolve from Column Name |
| ------------- | ------------- | ------------- | ----------- | ------- | ------------ | ------------------------ |
| User Lookup   | User Auth Key | sub           |             |         |              | Ext Auth Key             |
| Document      | Contact info  | given_name    |             | Contact | First Name   |                          |
| Document      | Contact Info  | family_name   |             | Contact | Last Name    |                          |
| Document      | Long User ID  | prefered_name |             | User    | Long User ID |                          |
| Template User | Template User | value         | *see below* |         |              | External Key             |

Template user Path: `{"url":"https://graph.microsoft.com/v1.0/users/{user}/memberOf?$select=id","jpath":"$..id"}`

![](assets/Pasted%20image%2020241018165543.png)


Xytech creates new users automatically when a valid OpenID user logs in. Xytech stores the users email address as it's unique identifier (in the above example the upn) into the external_key field of the user record.

-------
