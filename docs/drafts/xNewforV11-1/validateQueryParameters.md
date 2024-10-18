

Optional parameter

> [!NOTE] Note
> There is no standard scenario where this parameter is needed
> No need to include in documentation.



## validateQueryParameters = false

Prevents validating of the provided API call query parameters against the defined criteria set for the document.

When to use?
The system permits including query parameters that are not in the defined criteria set of fields.

Doing so, will incur an additional performance overhead, so please take that into consideration.

When using a list query, and you are passing a query parameter that is not contained in the criteria definition of that document.

Else if you define a query parameter that is not known, you will receive an error response of 'Undefined parameter', 
Example:
```json
{

    "error": "Undefined Parameter &#39;jm_job_JOBCUST_field_45&#39;. To allow undefined parameters pass validateQueryParameters = false"

}
```