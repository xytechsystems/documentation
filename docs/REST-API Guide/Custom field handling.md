---
title: Custom Field Handling
weight: 22
---
Any custom fields you have defined through document customization are automatically included in REST API definition. There is specific naming convention that is a combination of the Custom Code you created via document customisation plus the internal field name concatenated with an underscore. i.e. {customization code}\_{field name}

Document customization screen showing the user definable Customization Code
![](assets/Pasted%20image%2020240801134803.png)

_Document customization screen showing the internal field names:_
![](assets/Pasted%20image%2020240801134849.png)

Response from a GET call to fetch a Work Order that shows custom fields:
![](assets/Pasted%20image%2020240801134928.png)