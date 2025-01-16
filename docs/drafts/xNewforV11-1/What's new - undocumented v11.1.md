


------------
Undocumented:
## New Save Arguments
For JmActual

The correct response includes a prefix of the sub-table.
Currently V2 REST API 


Walt:
*There is, however, a problem with the way models (schemas) are generated and used in the Rest API.  There are cases where the payload returned in a get call and the payload expected for a post call are different.  When that happens the problem is actually a bug in the Rest API.  Swagger expects the same model to be used in both of those cases, but they differ because the post expects a model wrapped with the object name (like in your example) but the get returns the model without the object name wrapper.  Unfortunately, making them agree is a breaking change, so this is probably something that shouldn't be addressed until v3 of the API is created.*



## OpenID Authentication method
[19988](https://dev.azure.com/xytsystems/Xytech%20Platform/_workitems/edit/19988)
Can now obtain bearer token from auth provider and use that token to call the Xytech REST API.
### OpenID / Bearer Token (v11.1)
1. Obtain token from your OpenID auth provider 
Follow the steps provided by your auth provider e.g. Microsoft/Google

2. Use the token to call the REST API
Include token as a header in the format:
`'Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik1'`  

Note: Tokens will expire, so will need periodic refreshing.