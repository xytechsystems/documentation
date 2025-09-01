---
title: Custom Field Handling
weight: 22
---
## Custom Fields
Any custom fields you have defined through document customization are automatically included in REST API definition. There is specific naming convention that is a combination of the Custom Code you created via document customisation plus the internal field name concatenated with an underscore. i.e. {customization code}\_{field name}

Document customization screen showing the user definable Customization Code
![](assets/Pasted%20image%2020240801134803.png)

_Document customization screen showing the internal field names:_
![](assets/Pasted%20image%2020240801134849.png)

Response from a GET call to fetch a Work Order that shows custom fields:
![](assets/Pasted%20image%2020240801134928.png)


## Custom drop-down fields with additional attributes
Users can enable **custom dropdown fields** (through enabling a document customisation flag 'Additional API details', so that when called by the REST API will return the additional attributes stored with the dropdown record such as external_key.

Document Customisation fields showing additional checkbox:
![](assets/Pasted%20image%2020250328121552.png)

Example of a custom drop-down field when 'Additional API details' is <u>not</u> enabled:
![|334](assets/Pasted%20image%2020250820122610.png)
Example of a custom drop-down field when 'Additional API details' <u>is</u> enabled:
![|332](assets/Pasted%20image%2020250820122824.png)

