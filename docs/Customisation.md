# Customisation

## Custom Fields

### Introduction

Many Finastra products provide the capability for Banks to define additional custom fields alongside 
common fields for resources (business objects) such as parties, accounts, loans etc.

The following lists typical terms used to describe these additional
fields:

-   custom fields - this is the term used in this log
-   extension fields
-   additional information fields
-   user defined fields

Custom-field data extension capability is typically provided by back office systems as an 'out of the box' feature developed by
the product R&D team to allow each individual Bank to add their own custom fields e.g. for head office reporting.

For example, an endpoint of GET /accounts/{id} may need to return a set of a Bank's custom account fields along with the common Core account fields.
The custom fields are Bank specific and cannot be determined in advance i.e. they are dynamic.

It should be noted that the available set of custom fields can be described by a json schema - 
this schema represents a contract for any external developers and as such it has a lifecycle that
must be managed in the same way as APIs i.e. versioned and avoidance (where possible) of breaking changes.

> It is NOT expected that Open APIs will provide support for custom
> fields because of the increased complexity to the developer and
> support overheads for Finastra, however, there may be use cases for
> specific products where custom fields are a necessary requirement for
> an API.

###Sample Scenario

![Title](images/cutomization-sample-scenario.png)


### Rules Summary

- **MUST NOT** provide dedicated Bank specific APIs support data extension customization, use the technique below to avoid duplicating APIs
- Custom fields **MAY** be considered for API request
- When implemented, custom fields **SHOULD** be included in the payload of the associated business object i.e avoid providing a separate endpoint solely for custom-fields
- When implemented, custom fields **MUST** be grouped under the name ```custom-fields```
- When implemented, custom fields **SHOULD** use the ```additionalProperties``` keyword against the ```custom-fields``` field
- When implemented, custom field schemas **SHOULD** be provided as an endpoint in the associated business object API using the path name ```/custom-fields-schema```
as e.g ```GET /accounts/custom-fields-schema```
  
- When implemented, custom fields **MUST** be grouped under the name or prefix ```custom-fields```
- When implemented, custom fields schemas **MUST** adhere to [JSON Schema](https://json-schema.org/understanding-json-schema/basics.html)

### Solution

Custom field data extension can be supported in Finastra APIs by ensuring that clients using the API can obtain details of the expected custom fields for a specific Bank.

Open APIs are expected to define a fixed set of well known and documented fields for Fintechs which are defined in files adhering to the the OpenAPI Specification. 
Since custom fields are specific to a Bank then the customization breaks one of the fundamental FusionFabric principles which is 'code once , run everywhere'. 
This is why it is not recommended to push for customization within APIs. Note that most Open APIs on the market do not offer customization, however, 
Finastra APIs may need to support custom fields because Banks (especially top tier Banks) expect it to be implemented.

#### Endpoints

The following APIs and endpoints (as shown in the diagram) must be fully considered when implementing custom fields for a resource:

1. In the **resource API** ensure that ```POST```, ```PUT``` and ```GET``` support custom fields e.g ```GET /accounts/{id}``` will allow a **client** to retrieve common and 
   custom **data** fields and ```POST /accounts``` will create common and custom fields
2. In the **resource API** ensure that a client can ```GET``` the schema of the "customized part" e.g. ```GET /accounts/custom-fields-schema``` will allow a 
   **client** to retrieve the **schema** of the custom fields for the specific Bank (tenant)
3. In a **resource meta data API** consider supporting ```POST, PUT, GET, DELETE``` e.g. ```POST /accounts/custom-fields-schema``` will allow a **Bank** to define the custom field **schema** for a business object

![Title](images/customization-endpoints.png)


#### Example : Resource API - Client - DATA


>In the resource API ensure that POST, PUT and GET support custom fields e.g (1) ```GET /accounts/{id}``` 
>will allow a client to retrieve both common and custom data fields and (2) ```POST /accounts``` will create common and custom fields


##### Custom fields as Name-Value Pairs of Type String
The following list shows:

- First API definition schema, how an API would be defined to support name value pairs which are all strings
- Second Payload, how a sample payload might look


>**NOTES**
>- The custom-fields field defines the additionalProperties keyword as a string - this allows any additional properties to be included as long as they are a name-value pair that is also a string as per the example above
>- If all custom field name value pairs are of type integer the the type of the field custom-fields would be integer
>- The custom field names access_card, birth_date, monthly_income are NOT defined in the API because they are dynamic fields
>- The custom field names access_card, birth_date, monthly_income do NOT adhere to Finastra API field naming standards because they are not explicitly defined in the API, rather they are defined externally e.g. in the Core backend
>- For further details of the additionalProperties keyword see [JSON schema](https://json-schema.org/understanding-json-schema/reference/object.html#id2)



**API definition - schema**
```
account:
  type: object
  required:
  - name
  properties:
    number:
      type: string
      example: 0543123467083
    name:
      type: string
      example: Current account 
 
    custom-fields:
      type: object
      description : additional fields as defined by the financial institution, schema and
                    documentation can be found using the associated meta data endpoint
      additionalProperties:
        type: string
        example: { "custom-field-name": "custom-field-value" }
```
**Payload GET /accounts/{id}**

```
{
   "account": {
      "number": "0001000123012",
      "name": "Current account - 2020",
      "custom-fields": {
            "access_card": "1456",
            "birth_date": "1974-01-24",
            "monthly_income": "41.0"
             }
       }
}
```

##### Custom fields as Name-Value Pairs of Type Object

> This approach which uses an object rather than string is recommended

If a more complex structure is required, for example, a structure with a mix of data types and/or nested fields then the custom-fields field would be defined with the additionalProperties keyword as an object.

The following shows:

- First, how an API would be defined to support a more complex structure
- Second, how a sample payload might look - in this case the  monthly_income  is a number field NOT a string field

**API definition - schema**

```
account:
 
  ... etc. ...
 
    custom-fields:
      type: object
 
      description : additional fields as defined by the financial institution, schema and
                    documentation can be found using the associated meta data endpoint
      additionalProperties:
        type: object
        example: { "custom-field-name": "custom-field-value" }
```

**Payload GET /accounts/{id}**

```
{
   "account": {
      "number": "0001000123012",
      "name": "Current account - 2020",
      "custom-fields": {
            "access_card": "1456",
            "birth_date": "1974-01-24",
            "monthly_income": 41.0
             }
       }
}
```

#### Example for Resource API-CLIENT-SCHEMA

>In the resource API ensure that a client can GET the schema of the "customized part" e.g. 
> ```GET /accounts/custom-fields-schema``` will allow a client to retrieve the schema of the custom fields for the specific Bank (tenant)

**Schema payload**

To allow the external developer to obtain the schema for custom-fields a separate endpoint SHOULD be provided.

It is mandatory that custom field schemas adhere to Json schema.

The following table shows an example schema for Example 1 [Custom fields as Name-Value Pairs of Type Object](Customisation.md#custom-fields-as-name-value-pairs-of-type-object)

```
PAYLOAD GET /account/custom-fields-schema
{ 
    "$schema": "http://json-schema.org/draft-07/schema#",
    "$id": "http://json-schema.org/draft-07/schema#",
    "description": "Custom Fields schema for Account Business Entity",
    "type": "object",
    "required": [
        "access_card",
        "birth_date"
    ],
 
    "properties": {
        "access_card": {
            "type": "integer",
            "title": "Access Card Number",
            "default": "0"
        }, 
        "birth_date": {
           "type": "string",
            "format": "date",
            "title": "Date Of Birth"
        },
        "monthly_income": {
            "type": "number",
            "title": "Monthly Income",
            "default": 18000,
            "minimum": 15000.50,
            "maximum": 50000.45
        }
    }
}
```

**Schema of the Schema Payload**

The schema is shown in Swagger format to allow the contents to be viewed 
in the [Swagger Editor](https://editor.swagger.io/).
>It should be noted that the fb-retail-banking team could not get the below definition 
> to work with the Spring code generator. 
> To fix this they needed to add an additional 'properties' tag as per the following code snippet:


```
custom-fields-schema:
  type: object
  properties:
    type:
      type: string        <----- removed enum
    title:
      type: string
    description:
      type: string
    required:
      type: array
      items:
        type: string
    properties:     <----------------------- EXTRA - gives map(string, object)
      type: object
      additionalProperties:
        type: object
        properties:
          type:
            type: string   <----- removed enum
          description:
            type: string
          maxLength:
            type: integer
 
  example:
    type: object
    title: Custom Fields definition
    description: Custom Fields for Customer Information
    required:
      - customer
      - identification
    properties:
      customer:
        type: string
        description: Customer Name
        maxLength: 30
      identification:
        type: string
        description: identification
        maxLength: 30
```

#### Example - Resource Meta Data API-Bank-Schema

>In a resource meta data API consider supporting ```POST, PUT, GET, DELETE```
> e.g.```POST /accounts/custom-fields-schema``` will allow a Bank to define 
> the custom field schema for a business object

This is the same as example 2 except the target audience 
is the Bank who can POST, PUT and DELETE custom-field schemas.

It is anticipated that this endpoint would be included in a separate API and would not typically be required.

#### Supporting Multiple Schemas

This page has described scenarios for custom fields whose payload for a Bank can be described 
by a schema which is retrieved using a url similar to the following: 
```GET /accounts/custom-fields-schema```, where this endpoint would be used alongside 
another endpoint e.g:  ```POST /accounts``` to post common and custom account fields.

There are scenarios where multiple schemas for a specific resource are needed by a Bank, for example, 
the Finastra Fees module a LoB providing a fees API where ```resources="fees"``` and ```{schemaId}="loans"```
in this case the endpoints would be

| Endpoint Format                                      |      Endpoint description                                     |  Example                                | Example description |
|----------                                            |:-------------:                                                |------:                                  | ------:           |
| ```GET /resources/custom-field-schemas```            |  get all ```schemaIds``` associated with resources            | GET /fees/custom-fields-schemas         |get all ```schemaIds``` associated with fees |
| ```GET /resources/custom-fields-schema/{schemaId}``` |  get the ```schema``` for the specific schemaId               | GET /fees/custom-fields-schema/{loans}  |get the schema for ```{schemaId}="loans"``` |
| ```POST /resources```                                | create a ```resources``` object with common and custom fields | POST /fees/{loans}                      | create a fees loans object with common and custom fields  |
