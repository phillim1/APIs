# Collections and Pagination

This section details the following topics regarding collection handling
and pagination:

-   Sorting
-   Searching and Filtering
-   Pagination Request
-   Pagination Response
-   HATEOAS and HAL
-   Dynamic Response Payloads

Pagination should be provided within Finastra APIs when an API call
returns a collection of objects.

Pagination should be introduced early in an API’s lifecycle because
pagination typically introduces breaking changes. In addition, if
pagination is not introduced the API may list an entire collection of a
resource which may represent an SLA risk, a security risk, a resiliency
concern etc.

> Finastra APIs with resource collections **SHOULD** support pagination
> in all but exceptional circumstances.

## Sorting

When a Finastra API provides a `GET` endpoint that returns a collection
the API should provide sort capabilities as described in this section.

> Finastra APIs containing resource collections **SHOULD** support sort
> capability.

> Finastra APIs supporting sort **MUST** use the **sort** path keyword
> with `asc` and `desc` parameters where `desc` means reverse order and
> `asc` means ascending order, for example:

``` notoggle
($propertyname,)+[asc|desc]
```

> Finastra APIs supporting sort **MUST** specify the default sort
> sequence in API documentation.

> Finastra APIs supporting sort **MUST** specify the maximum number of
> sort terms.

**Examples**

-   Sort by `id`, in ascending order:

``` notoggle
GET /accounts?sort=id  
```

-   Sort by `openDate`, in descending order:

``` notoggle
GET /accounts?sort=openDate+desc
```

-   Sort by `openDate`, in descending order, and then by `name`, in
    ascending order

``` notoggle
GET /accounts?sort=openDate+desc,name+asc
```

## Searching and Filtering

When a Finastra API provides a `GET` endpoint that returns a collection
the API should provide search and filter capabilities to avoid
performance issues when a potentially large collection are returned.

Search and filter capabilities can be supported using:

1 - basic mechanism - with query parameters specified in the url which
contain the resource’s filter field name and filter field value

2 - advanced mechanism - as per the basic mechanism but additionally
implementing a query language to allow more advanced search capabilities
e.g. using query languages similar to:

-   [RSQL](https://github.com/jirutka/rsql-parser)
-   [FIQL](https://tools.ietf.org/html/draft-nottingham-atompub-fiql-00)
-   [GraphQL](https://graphql.org/)
-   [OData](https://www.odata.org/)

**Examples**

``` notoggle
* GET /accounts?name=tom&lastname=smith     = > basic using query parameter
* GET /accounts/search/mostActive           = > well known search
* GET /accounts/search/balance=lt=0         = > using FIQL
```

**Finastra API Search and Filter Standards**

> Support for searching and filtering **SHOULD** be provided using GET
> and query parameters to allow searches to be bookmarked

> RSQL, FIQL or OData syntax **MAY** be used for the GET on the search
> resource

> GraphQL **SHOULD NOT** be the default choice for API style

> The choice of technology will be based on functional requirements

> POST endpoints **MAY** be implemented for complex searches where query
> language is included in the request payload

## Pagination Request

When a Finastra API provides a `GET` endpoint that returns a collection
the API should provide capabilities to allow the client to page through
the collection.

There are two commonly used pagination techniques:

-   Offset-Limit Based pagination - this uses a numeric offset to
    identify the first item in a set of results - this is the most
    widely used pagination mechanism

-   Cursor Based (Key-Based) pagination - this uses a unique key to
    identify the first item in a set of results - this is typically used
    for data that changes frequently

> Finastra APIs supporting pagination **SHOULD** support offset / limit
> based pagination

Note that there may be specific use cases that require cursor based
pagination to be implemented.

In Offset-Limit Based pagination:

-   the **limit** parameter controls the maximum number of items that
    may be returned for a single request

-   the **offset** parameter controls the starting point within the
    collection of results, for example, if a collection of 15 items is
    to be retrieved from a resource and a limit=5 is specified then the
    set of results can be retrieved in 3 requests by varying the offset
    value: `offset=0`, `offset=5`, and `offset=10`

**Examples**

``` notoggle
* `GET /accounts?limit=`100`  => return the 100 first accounts 
* `GET /accounts?offset=100`  => return accounts from 101 
```

> Finastra APIs supporting pagination **SHOULD** be implemented using
> the **limit** and **offset** request keywords

## Pagination Response

This section provides details on the contents of the response object for
a collection of items and includes the following:

-   Return Items - as an Object containing an Array of items
-   Return Metadata - with pagination navigation information
-   Return Links - with associated relevant links

### Return Items

When a Finastra API provides a `GET` endpoint that returns a collection
e.g. GET /accounts, the API should provide a response object that is
extensible, hence, a collection should be returned as an object
containing an array of items - for example:

``` notoggle
GET /accounts 
```

``` notoggle
{
  "items": [
    {
      "id": "0001123467001",
      "name": "Current GBP",
      "uri": "/accounts/0001123467001"
    },
    {
      "id": "0001123467002",
      "name": "Current EUR",
      "uri": "/accounts/0001123467002"
    },
    {
      "id": "0001123467003",
      "name": "Current USD",
      "uri": "/accounts/0001123467003"
    }
  ]
}
```

> Finastra APIs supporting `GET` collections **SHOULD** return the
> collection as an Object containing an Array of items because this
> approach can extend to support meta data without breaking changes

-   see the next section for extending the response object

> Finastra APIs supporting GET collections **SHOULD** support
> extensibility using an object with the `items` keyword

> Finastra APIs supporting **simple and unchanging** `GET` collections
> **MAY** use arrays to return the collection

### Return Metadata

The previous section showed how a collection can be returned as an
object named *items* containing an array of items in the collection.

By returning *items* as an object the response can be extended, without
breaking changes to the API, to return an object named \_\_meta\_
containing metadata associated with the collection.

The following example shows the standard Finastra pagination metadata
response fields:

``` notoggle
{
"_meta" :
  {
    "limit" : 5         // maximum number of items returned  
    "offset" : 60       // starting point of items returned 
    "itemCount" : 3     // total number of items returned
    "totalCount" : 63   // total number of items 
  }
}
```

The \_\_meta\_ object can be extended to include any relevant metadata
as required e.g. `computeTime`.

The following example shows *items* and \_\_meta\_ in a typical response
object:

``` notoggle
GET /accounts?limit=5&offset=60
```

``` notoggle
{
  "items": [
    {
      "id": "0543123467009",
      "name": "Current GBP",
      "uri": "/accounts/0543123467009"
    },
    {
      "id": "0543123467010",
      "name": "Current EUR",
      "uri": "/accounts/0543123467010"
    },
    {
      "id": "0543123467083",
      "name": "Savings GBP",
      "uri": "/accounts/0543123467083"
    }
  ],
  "_meta": {
    "limit": 5,
    "offset": 60,
    "itemCount": 3,
    "totalCount": 63
  }
}
```

> Finastra APIs supporting GET collections **SHOULD** support the
> provision of metadata using the `_meta` keyword

### Return Links

When a Finastra API provides a `GET` endpoint that returns a collection
the API should provide the client with the following links to navigate
the collection:

-   **self** - this link will return the current set of items in the
    collection
-   **next** - this link will return the set of items in the collection
    after the last entry of the current set of items
-   **prev** - this link will return the set of items in the collection
    before the first item of the current set of items
-   **first** - this link will return the first set of items in the
    collection
-   **last** - this link will return the last set of items in the
    collection

All items are based on the passed limit and offset parameters.

Note that links can also be provided for GET operations that return a
single item rather than a collection - see the *HATEOAS and HAL* section
for further details.

The following example shows a Swagger snippet containing the standard
definition of links:

``` notoggle
  PaginationLinks:
    type: object
    required:
      - self
    properties:
      next:
        $ref: '#/definitions/GenericLink'
      last:
        $ref: '#/definitions/GenericLink'
      prev:
        $ref: '#/definitions/GenericLink'
      self:
        $ref: '#/definitions/GenericLink'
      first:
        $ref: '#/definitions/GenericLink'

  GenericLink:
    type: object
    description: Hypertext reference
    required:
      - href
    properties:
      href:
        type: string
        description: URI to be used
```

The following example shows how *items* and \_\_meta\_ and \_\_links\_
can be used together:

``` notoggle
GET /accounts?limit=5&offset=60
```

``` notoggle
{
  "items": [
    {
      "id": "0543123467009",
      "name": "Current GBP",
      "uri": "/accounts/0543123467009"
    },
    {
      "id": "0543123467010",
      "name": "Current EUR",
      "uri": "/accounts/0543123467010"
    },
    {
      "id": "0543123467083",
      "name": "Savings GBP",
      "uri": "/accounts/0543123467083"
    }
  ],
  "_meta": {
    "limit": 5,
    "offset": 60,
    "itemCount": 3,
    "totalCount": 63
  },
  "_links": {
    {
      "self": "/accounts?limit=5&offset=60"
    },
    {
      "first": "/accounts?limit=5&offset=0"
    },
    {
      "previous": "/accounts?limit=5&offset=55"
    },
    {
      "last": "/accounts?limit=5&offset=60"
    }
  }
}
```

Note that in the example, since the last 3 items are shown based on the
request parameters then the “next” link is omitted from the response.

> Finastra APIs supporting GET collections **SHOULD** support navigation
> using the `_links` keyword

## HATEOAS and HAL

[HATEOS](https://en.wikipedia.org/wiki/HATEOAS) refers to Hypermedia as
the Engine of Application State which is a style whereby API responses
provide links that are related to the API requested by a client, for
example, if a client requests a resource such as an account then the
response might return a set of links specifying how to obtain additional
details of the account such as balances and transactions. The links
returned by the response will depend on the capability of the API and
the scenarios in which it is exepected to be used.

> Finastra APIs **SHOULD** ensure that relevant links are returned where
> appropriate.

[HAL](https://en.wikipedia.org/wiki/Hypertext_Application_Language)
refers to Hypertext Application Language and it is a standard convention
for defining links to external resources within JSON or XML code, for
example, if a client requests a resource such as an loan then the
response might return a set of links in HAL format specifying how to
obtain additional details of the loan.

HAL can be used, however, it is currently defined as a [draft
proposal](https://tools.ietf.org/html/draft-kelly-json-hal-08).

HAL uses a strict JSON structure based on `_links` and `_embedded`
objects.

> Finastra APIs **MAY** use HAL syntax for the response structure

Note that HATEOAS and HAL are not limited to GET operations with
collections, they can be used in other scenarios, for example, GET
/acccounts/0543123467083 might return links to balances and transactions
as shown in the following example:

``` notoggle
GET /accounts/0543123467009
```

``` notoggle
{
   "account": {
      "accountId": "0543123467009",
      "name": "Current"
      "currency": "GBP",
      "type": "Current - Basic"
      },
      "_links": [
         {
            "rel": "balances",
            "href": "/accounts/0543123467009/balances"
         },
         {
            "rel": "transactions",
            "href": "/accounts/0543123467009/transactions"
         }
      ]
   }
}
```

## Dynamic Response Payloads

When a client knows that an API payload is large but the client is only
interested in a subset of the payload fields, then the API may provide a
way to allow the client to specify the list of fields to return.

Note that there is no native facility within `OAS v2.0/v3.0` to support
dynamic request/response bodies.

This section details the following mechanisms for supporting dynamic
response payloads:

-   1 - basic - list fields in the url
-   2 - intermediate - define names for blocks of fields and identify
    these in the url

The following is a summary of the Finastra Dynamic Response Payloads
Standards:

> Field selection for large payloads **MAY BE** supported, however,
> beware complexity of the implementation and the risk to meeting SLAs

> Field selection **SHOULD** be implemented using the **fields** keyword
> or a **style** keyword in the url query parameter

> If field selection is implemented, responses to requests for partial
> payload content **SHOULD** return a 206 HTTP status code

### Name Fields

1 - The field selection capability to provide dynamic response payloads
can be provided by allowing the client to specify a list of fields in
the url.

For example, a request to return only the requested fields:

``` notoggle
GET: https://{host}/accounts?fields=name,customerName,openDate
```

Would be expected to respond as follows:

``` notoggle
{
  "name": "...",
  "customerName": "...",
  "openDate": "..."
}
```

### Name Field Blocks

The field selection capability to provide dynamic response payloads
extends the `fields` keyword by allowing the client to specify a name
that identifies a set of fields.

For further details see: [NBB - Filtering Using
Styles](https://github.com/NationalBankBelgium/REST-API-Design-Guide/wiki/Filtering-Using-styles)

For example, a request to return only the balances for an account can be
implemented using a selection where a `fieldBlock` keyword identifies
the fields to return:

``` notoggle
GET: https://{host}/accounts?fieldBlock=balances
```

Would be expected to respond as follows:

``` notoggle
{
  "accountId": "0543123467083",
  "accountBalances": [
    {
      "type": "AVAILABLE",
      "asofDate": "2018-12-20",
      "amount": {
        "amount": "-5877.78",
        "currency": "GBP"
      }
    }
  ]
}
```