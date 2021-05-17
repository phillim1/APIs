Finastra Open API Design Guide
================
**Fusion**Fabric.cloud Documentation Team
13 April 2021

-   [](#frontpage)
-   [(PART\*) Design Guidelines](#part-design-guidelines)
-   [Introduction to REST](#introduction-to-rest)
    -   [REST](#rest)
    -   [REST Resources](#rest-resources)
    -   [REST and HTTP Transport](#rest-and-http-transport)
    -   [REST and HTTP Methods](#rest-and-http-methods)
    -   [REST and HTTP Headers](#rest-and-http-headers)
-   [Introduction to Open APIs](#introduction-to-open-apis)
    -   [Open APIs and OpenAPI](#open-apis-and-openapi)
    -   [Finastra Open API Principles](#openapi-principles)
    -   [API Maturity Levels](#api-maturity-levels)
-   [Payloads and Formats](#payloads-and-formats)
    -   [JSON Payloads](#json-payloads)
    -   [Other Payloads](#other-payloads)
    -   [XML and Other Standard
        Formats](#xml-and-other-standard-formats)
-   [Responses and Errors](#rest-error-handling)
    -   [Finastra Error Message
        Structure](#finastra-error-message-structure)
-   [Naming Conventions](#naming-conventions)
    -   [Resources](#resources)
    -   [Resource Payload](#resource-payload)
    -   [Abbreviations](#abbreviations)
    -   [Enumerations](#enumerations)
-   [Versioning](#versioning)
    -   [Backward compatibility](#backward-compatibility)
    -   [REST versioning](#rest-versioning)
-   [Collections and Pagination](#collections-and-pagination)
    -   [Sorting](#sorting)
    -   [Searching and Filtering](#searching-and-filtering)
    -   [Pagination Request](#pagination-request)
    -   [Pagination Response](#pagination-response)
    -   [HATEOAS and HAL](#hateoas-and-hal)
    -   [Dynamic Response Payloads](#dynamic-response-payloads)
-   [Bulk Operations](#bulk-operations)
-   [Server Events and Long Running
    Tasks](#server-events-and-long-running-tasks)
    -   [Long-Polling](#long-polling)
    -   [Webhooks](#webhooks)
-   [Customisation](#rest-customisations)
    -   [Localisation and
        Internationalization](#localisation-and-internationalization)
    -   [Custom Fields](#custom-fields)
-   [Concurrency](#concurrency)
-   [Idempotency](#idempotency)
-   [(PART\*) Appendix](#part-appendix)
-   [Glossary](#glossary)
-   [Finastra Standards Summary](#standards-summary)
    -   [Conventions](#conventions)
    -   [Principles](#principles)
    -   [General](#general)
    -   [Names](#names)
    -   [HTTP Methods](#http-methods)
    -   [HTTP Headers](#http-headers)
    -   [HTTP Custom Headers](#http-custom-headers)
    -   [Swagger info - API
        Information](#swagger-info---api-information)
    -   [Swagger host - Serving the
        API](#swagger-host---serving-the-api)
    -   [Swagger security - Security](#swagger-security---security)
    -   [Swagger paths - Operations and
        Resources](#swagger-paths---operations-and-resources)
    -   [Swagger parameters -
        Parameters](#swagger-parameters---parameters)
    -   [Swagger responses](#swagger-responses)
    -   [Swagger tags](#swagger-tags)
    -   [Swagger externalDocs](#swagger-externaldocs)
    -   [Other - Pagination](#other---pagination)
    -   [Other - Performance](#other---performance)
-   [Finastra Open API Dictionary](#rest-dictionary)
    -   [General Guidelines](#general-guidelines)
    -   [Specific Examples](#specific-examples)
-   [Useful Resources](#useful-resources)
-   [Document History](#document-history)

# 

### Welcome

This document is the **Finastra Open API Design Guide** and the intended
audience are designers and developers of Finastra Open APIs.

This document introduces the concepts of Finastra RESTful Open APIs and
details associated Finastra Open API development standards.

Designers and developers of Finastra Open APIs should read and
understand the concepts and standards included in this guide before
developing a Finastra API.

Finastra strategy is **API as a Product** and **API First** and these
approaches will lead to successful implementation of a Finastra Open API
as a product.

The Appendix of this document includes the following sections that are
relevant to both novice and advanced designers and developers of
Finastra APIs:

-   a glossary of terms used in the document
-   a quick reference summary of Finastra Open API standards
-   a dictionary containing code snippets as building blocks for
    Finastra APIs
-   patterns and resources applicable to Finastra APIs

<span class="text-title">**Document Conventions**</span>

-   The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”,
    “SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in
    Finastra standards are to be interpreted as described in [RFC
    2119](https://www.ietf.org/rfc/rfc2119.txt)

-   When a Finastra Open API standard is highlighted within the
    sections, the standard is shown in the following format:

> Finastra Open API Standards **SHALL** be documented in this style

<!--
[**Authors**]{.text-title}

This document is authored by:

+ [François Lasne](mailto:francois.lasne@finastra.com)
+ [Martin Phillips](mailto:Martin.Phillips@finastra.com)
+ [Mihai Terente](mailto:Mihai.Terente@finastra.com)

To reach out to the Open API team, send an email to openapi@finastra.com. 
-->
<script>
$(document).ready(function () {
  let title = document.getElementsByTagName("h3");
  title[0].style.display = 'none';
});
</script>
<!--chapter:end:index.Rmd-->

# (PART\*) Design Guidelines

# Introduction to REST

<!-- # Introduction to REST -->

Finastra’s technical strategy is to adopt a REST(ful) architectural
style by publishing Open APIs that use HTTP for communication to access
or update resources, such as accounts, loans, deposits etc.

This section provides an overview of the main components of REST APIs:

-   REST
-   REST Resources
-   REST and HTTP
-   REST and HTTP Methods
-   REST and HTTP Headers

## REST

REST stands for [REpresentation State Transfer
(REST)](https://en.wikipedia.org/wiki/Representational_state_transfer).

REST is an *architectural style* for providing *interoperability*
between systems.

REST is a concept and not a definitive standard.

REST APIs allow API consumers to execute a predefined set of stateless
operations on resources e.g. create, read, update or delete (CRUD) an
account

The following six guidelines (constraints) are used to define a RESTful
system:

-   Uniform Interface - based on resources
-   Client–Server architecture
-   Stateless
-   Layered
-   Cacheable
-   Code on Demand (optional)

## REST Resources

*The key abstraction of information in REST is a resource* - **Roy
Fielding**.

Any information that can be named can be a resource:

-   a document
-   an image
-   a bank account
-   a temporal service e.g. “today’s weather in Los Angeles”
-   a collection of other resources
-   a non-virtual object e.g. a person
-   etc.

REST resources should reflect the business domain and should have
identifiers (keys) that change as infrequently as possible. Finastra
APIs will define resources aligned with the Banking business domain
e.g. currencies, accounts etc.

## REST and HTTP Transport

REST architecture supports the creation and maintenance of resources
using HTTP and associated Uniform Resource Identifiers (URIs).

HTTP is a protocol used for communication and provides capabilities for
transport versioning, content encoding, extensibility, upgradeability,
and security.

One of the goals of RESTful architecture is to support the gradual and
fragmented deployment of changes within an already deployed architecture
which is why REST and HTTP fit well together.

Finastra Open APIs use HTTP 1.1
([RFC7231](https://tools.ietf.org/html/rfc7231)) as a reference
implementation.

Finastra Open APIs may use HTTP 2
([RFC7540](https://tools.ietf.org/html/rfc7540)).

Transport Layer Security (TLS), formerly Secure Socket Layer (SSL),
secures message communication over a computer network by encrypting the
message contents. An HTTPS transfer or API call is an HTTP call over a
connection secured by TLS

> Finastra Open APIs **MUST** use HTTPS and TLS
> i.e. [RFC2660](https://tools.ietf.org/html/rfc2660) and
> [RFC2818](https://tools.ietf.org/html/rfc2818) are enforced

<!-- ## HTTP Methods -->

## REST and HTTP Methods

This section details the HTTP methods used by Finastra Open APIs.

A key requirement for REST APIs is the use of a resource with
appropriate HTTP verbs.

The HTTP verbs are defined in section 4.3 of
[RFC7231](https://tools.ietf.org/html/rfc7231#section-4.3). and those
relevant to Finastra are shown in the table below.

### Summary of HTTP Methods

The following table shows the HTTP methods and their expected actions on
resources:

| HTTP Methods | Action                                                                                          |
|--------------|-------------------------------------------------------------------------------------------------|
| **POST**     | Creates a new resource                                                                          |
| **GET**      | Reads existing resource(s) - does not change the state of the resource                          |
| **PUT**      | Updates a resource                                                                              |
| **PATCH**    | Partially updates a resource (typically avoided)                                                |
| **DELETE**   | Deletes a resource                                                                              |
| **HEAD**     | Identical to **GET**, except that the server **must not** return a message body in the response |
| **OPTIONS**  | Allows the client to determine the options and/or requirements associated with a resource       |

Most Finastra APIs will use the POST, GET, PUT and DELETE HTTP methods
to maintain business domain resources.

The remainder of this section details each HTTP method with reference to
the following associated characteristics which should be carefully
considered when designing the Finastra API:

-   Request Payload: specifies whether a request payload is required
-   Successful response payload: specifies whether a response payload is
    required
-   Safe: specifies whether an operation is safe; a safe operation does
    not change a resource’s representation on the server e.g. GET is
    considered safe
-   Idempotent: specifies whether an operation is idempotent; an
    operation is idempotent if it can be called several times with the
    same outcome
-   Cacheable: specifies whether an operation is cacheable; cached data
    may be used as the response to the same subsequent request

### GET

RESTful APIs use the `GET` method to obtain data from a resource.

`GET` will typically be included in most Finastra APIs as it is the only
query method.

**Examples**

-   `GET /customers/{id}` returns the customer identified with `{id}`.
-   `GET /customers` returns a list of all customers.

> As per the examples, Finastra APIs **SHOULD** implement `GET` methods
> against both an individual resource and its collection

> `GET` methods against a collection **SHOULD** implement pagination and
> search capabilities - see the *Navigation* section for details

**Characteristics**

-   Request Payload: No
-   Successful Response Payload: Yes
-   Safe: Yes
-   Idempotent: Yes
-   Cacheable: Yes

**Typical response status codes**

-   `200` - OK - returned when the client call is successful
-   `400` - Bad Request
-   `404` - Not Found

**Others characteristics of GET**

-   can be cached
-   saved in the browser history
-   can be bookmarked
-   has length restrictions (usually 2K)
-   should be used only to retrieve data

### POST

The `POST` method is used to create new resources. Depending on the
specific API context this can mean many things e.g., creating a
resource, executing an action, submitting long running operations,
adding an item to a list.

> APIs **MUST** include a response to clients that shows how the client
> can access the newly created resource

**Example**

``` notoggle
`POST /customers' with '{payload}`
```

**Characteristics**

-   Request Payload: Yes
-   Successful response payload: Yes
-   Safe: No
-   Idempotent: No
-   **POST** requests are never cached

> APIs **MUST** inform the client of the API the location of the newly
> created resource using the `Location` header

**Typical response status code**

-   `200` - OK - returned when the client call is successful - 201 is
    preferred when resources are created using `POST`
-   `201` - Created - this is returned with a location header and the
    created resource which should contain the id and data in the
    response payload
-   `400` - Bad Request - this is used for **both technical and
    functional** errors that did not result in a successful response
-   `409` - Conflict - this is returned when a request to create a
    resource is made and the resource already exists

### PUT

The `PUT` method is used to replace the state of an existing resource
with the new state provided in the request. It is a *full replacement*.

**Examples**

``` notoggle
* `PUT /customers/123' + '{payload}` - this requests an update to the resource
* `PUT /customers/' + '{payload}` - this requests a batch update
```

> Bulk requests **SHOULD** be avoided unless absolutely necessary;
> consider alternative designs

**Characteristics**

-   Request Payload: Yes
-   Successful response payload: No
-   Safe: No
-   Idempotent: Yes
-   Cacheable: No

**Typical response status codes**

-   `200` - OK - this **MUST** be returned when the client call is
    successful
-   `400` - Bad Request - this is used for **both technical and
    functional** errors that did not result in a successful response
-   `404` - Not Found

### PATCH

The `PATCH` method is used to partially update an existing resource.
Only the information provided in the request is used to update the
resource.

`PATCH` and `PUT` are both related to content update, and can been seen
as overlap.

It should not according to the
[RFC5789](https://tools.ietf.org/html/rfc5789), PATCH comes with a
defined semantic. The `PATCH` method requests that a **set of changes**,
described in the request entity, must be applied to the resource
identified by the request’s URI. This set contains instructions
describing how a resource currently residing on the origin server should
be modified to produce a new version. To implements correctly the set of
changes, PATCH is couple with the [JSON
Patch](https://tools.ietf.org/html/rfc6902) RFC

**Example**

``` notoggle
PATCH /customers/123` - only a partial view of the resource is required => return expected the full patched resource
```

``` notoggle
[  { "op": "replace", "path": "/email", "value": "new.email@example.org" }   ]
```

> ‘PATCH’ **SHOULD** be avoided as far as possible

**Characteristics**

-   Request Payload: Yes
-   Successful response payload: Yes
-   Safe: No
-   Idempotent: No
-   Cacheable: No

**Typical response status codes**

-   `200` - OK - this **MUST** be returned when the client call is
    successful
-   `400` - Bad Request - this is used for **both technical and
    functional** errors that did not result in a successful response
-   `404` - Not Found

### DELETE

The `DELETE` method is used to remove a resource. Depending on the
context of the resource this can have different meanings e.g.:

-   remove the resource
-   flag a resource for deletion
-   remove an item from a list
-   cancel a long running operation
-   etc.

**Example**

``` notoggle
`DELETE /customers/123467`
```

**Characteristics**

-   Request Payload: No - typically
-   Successful response Payload: No
-   Safe: No
-   Idempotent: Yes
-   Cacheable: No

> When N DELETE requests are invoked for the same resource, the first
> request will delete the resource and the response will be 200 (OK) or
> 204 (No Content). The subsequent N-1 requests will return 404 (Not
> Found), hence, the N responses differ which might be seen as
> non-idempotent, however, because there is no change of state for any
> resource on the server side and because the original resource is
> deleted by the first request, then DELETE can be deemed to be
> idempotent.

**Typical response status code**

-   `200` - OK - this **MUST** be returned when the client call is
    successful
-   `204` - No content - this **MAY** be used when there is no resource
    to delete
-   `400` - Bad Request - this is used for **both technical and
    functional** errors that did not result in a successful response
-   `404` - Not Found

### HEAD

The `HEAD` method may be used to retrieve headers which describe a
resource or resource collection. The response contains only the headers,
it does not contain resource information. An example of the usage of a
`HEAD` request is to inform on the size of a resource before download.

**Characteristics**

-   Request Payload: No
-   Successful response Payload: No
-   Safe: Yes
-   Idempotent: Yes
-   Cacheable: Yes

### OPTIONS

The `OPTIONS` method may be used to query the resource service to obtain
a list of the allowed HTTP communication options. This method may be
used to discover which HTTP operations are supported by an API.

**Characteristics**

-   Request Payload: No
-   Successful response Payload: Yes
-   Safe: Yes
-   Idempotent: Yes
-   Cacheable: No

### Other Actions

`Actions` are business capabilities that do not directly map to an
existing HTTP method. These may be anything from a simple stateless
function, without a resource representation, to complex workflows which
interact with multiple resources.

**Examples**

*Resource-less Actions*

``` notoggle
POST: https://{host}/atm/transfer-funds
```

``` notoggle
{
  "account-1": "...",
  "account-2": "...",
  "amount": "...",
  ...
}
```

> Resource-less actions **SHOULD** be avoided by using a relevant
> resource e.g. use POST /atm/funds-transfer

<span class="text-title">**Resource-based Actions**</span>

``` notoggle
POST:https://{host}/accounts/{id}/close-account
```

> Resource-based actions **SHOULD** be avoided by using a relevant
> resource e.g. use POST accounts/{id} with { status : closed }

### Finastra HTTP Methods Standards

-   **MUST** use the standard GET, POST, PUT, DELETE, PATCH methods
    against a resource.
-   **MUST** fully consider relevant and necessary resources when using
    HTTP methods.
-   **MUST NOT** use POST if GET can be used.
-   **MUST** handle a single resource {id} in the path.
-   **SHOULD NOT** use verbs within paths
    e.g. `POST /resources/{resourceId} {"status" : "active"}` rather
    than `POST /resources/{resourceId}/activate`
-   **SHOULD NOT** use constructs such as `GET /accounts?id=12` except
    for filtering - prefer `GET /accounts/12`for retrieval of a single
    resource.
-   **SHOULD** ensure API operations on resources are complete in their
    CRUD scope.
-   **SHOULD** ensure that non RESTful actions are not used.
-   **MAY** use HEAD and OPTIONS methods.

## REST and HTTP Headers

This section provides details on the standard and custom HTTP headers
that may be used with Finastra Open APIs.

### Standard HTTP Headers

| Header          | Description                                                                                                                                                                                                                                                                                            |
|-----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `Content-Type`  | Represents the format of the payload returned in the response. <BR>Content-type MUST be `application/json`, as a content header in response to requests that return a HTTP body i.e. all POST and GET requests.                                                                                        |
| `Accept`        | Represents the format of the payload as input.<BR>Accept SHOULD be `application/json`.                                                                                                                                                                                                                 |
| `Authorisation` | Allows Credentials to be provided to the Authorisation / Resource Server depending on the type of resource being requested.<BR> For OAuth 2.0 / OIDC, this comprises of either the Basic / Bearer Authentication Schemes. Bearer token MUST follow the [RFC6570](https://tools.ietf.org/html/rfc6750). |

> APIs **MUST** support a `Content-Type` of `application/json`

### Custom HTTP Headers

Custom headers **MAY** be used to add extra functionality which is
exposed via HTTP. All usage should be confirmed with the Finastra API
team.

> Custom Headers **MUST** be in `Train-Case` if used

> X-Request-ID **SHOULD** be used for tracking requests

# Introduction to Open APIs

Systems expose APIs to allow users and other systems to interact with
them.

APIs are the contracts which act as glue between systems.

A Finastra API should be designed so that:

-   it exposes functionality in a manner that is understood by
    developers with domain knowledge rather than knowledge of the
    underlying system
-   it is future proofed as far as possible so that it does not require
    significant future changes because once an API has been published it
    becomes difficult to change
-   it hides the internal implementations of the system it exposes

## Open APIs and OpenAPI

<span class="text-title">**Open API**</span>

From [Wikipedia](https://en.wikipedia.org/wiki/Open_API): *“An open API
(often referred to as a public API) is a publicly available application
programming interface that provides developers with programmatic access
to a proprietary software application or web service”*

<span class="text-title">**OpenAPI**</span>

The most common format to describe Open APIs is the [**OpenAPI
Specification (OAS)**](https://www.openapis.org/) standard which
describe the structure and syntax of the document, or code, that is used
to describe REST Open APIs.

Swagger is tooling that allows developers to design build and document
Open APIs according to the OpenAPI Specification.

An extensive OSS community provides tools for OAS APIs such as codegen,
editors, and docgen.

The Open API definition is agnostic to the server implementation in that
the underlying services can be written in various stacks - .NET, Spring,
etc., without change to the Open API. Similarly, the underlying services
must be agnostic of clients calling their APIs.

A well written API will allow external API developers to use the
contents of the API definition to develop their own client without
recourse to further documentation. This requires the API documentation
to contain full details of data structures, validation rules and
expected behaviors for the operations within the API.

**References**

-   [Open API Initiative](https://www.openapis.org/)
-   [OpenAPI
    v2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)
-   [OpenAPI
    v3.0.0](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.0.md)
-   [Swagger Editor](https://editor.swagger.io/)

------------------------------------------------------------------------

Within Finastra the following rules apply:

> Finastra Open APIs **MUST** be defined using the OpenAPI Specification
> (OAS)

> Finastra Open APIs **MUST** be well documented

> Finastra Open APIs **SHOULD** adopt API First and API as a Product
> strategies

> Finastra Open APIs **MAY** be be defined using OpenAPI v2 or v3

## Finastra Open API Principles

The following principles are specific to the Finastra Open API
initiative and reflect the strategic goals when designing and developing
Finastra Open APIs.

The audience for these principles is API designers and developers with
focus on the quality attributes of the API definition and the underlying
API implementation.

The Open API initiative adheres to the broader organisation architecture
principles.

The Open API principles are:

-   [Useful](#openapi-principle-useful)
-   [Keep it Simple](#openapi-principle-keepitsimple)
-   [Easy to Use](#openapi-principle-easytouse)
-   [Consistent](#openapi-principle-consistent)
-   [Backward Compatible](#openapi-principle-backwardcompatible)

It is assumed that an **API First** approach is adopted to deliver
**APIs as Products**.

### Useful

> Finastra Open APIs must be useful.

**Rationale**

Finastra Open APIs MUST be designed based on client requirements. They
should contain as much information as necessary but as little as
possible. They can support multiple related use cases.

**Implications**

Finastra Open API designers must ensure that the API’s target audience
and use cases are well known so that they can fulfill product
requirements.

Finastra Open API designers will adopt an iterative approach to design
ensuring that design fails fast, feedback is obtained and the API is
continually improved until the API is deemed useful and of production
quality.

### Keep it Simple

> Finastra Open APIs must be designed to be as simple as possible.

**Rationale**

Finastra Open APIs will be publicly accessible and will be used
primarily by external API developers. The successful adoption of
Finastra APIs will be determined by API developers’ experience.

API developers will adopt and promote Finastra Open APIs that are
simple.

**Implications**

Finastra Open API designers MUST assume that the audience are:

-   NOT Finastra product experts - do NOT use product specific
    vocabulary.
-   NOT Banking experts - do use known Banking terminology.
-   NOT Technical experts - do NOT use technical jargon.

### Easy to Use

> Finastra Open APIs must be designed to be (1) easy to use and (2) hard
> to misuse.

**Rationale**

Finastra Open APIs will not be adopted if they pose any barriers to API
developers.

**Implications**

Finastra Open API designers MUST ensure that the API is:

-   Clear
-   Concise
-   Coherent
-   Unambiguous
-   Intuitive to use
-   Consistent

### Consistent

> All Finastra Open APIs must be consistent in vocabulary and behaviour.

**Rationale**

Finastra Open APIs must be professional:

-   They MUST be production-quality ready.
-   They MUST have a look and feel that is consistent across all
    Finastra Open APIs.

**Implications**

Finastra Open API designers and developers MUST ensure that:

-   Finastra standards are followed.
-   Common definitions are reused where appropriate.
-   Open APIs are created using linters that use Finastra Swagger rules
    to validate.
-   Open APIs are validated as part of CI/CD using Finastra Swagger
    rules.
-   Open APIs are approved for publication by the Open API team.

### Backward Compatible

> All Finastra Open APIs must be backward compatible between releases.

**Rationale**

Once a client has adopted a Finastra Open API the client expects that
API to function correctly on all future releases unless the client is
explicitly told that the API is being deprecated and obsoleted

**Implications**

Finastra Open API designers MUST ensure that:

-   APIs are versioned
-   Breaking changes to an API are not introduced

## API Maturity Levels

The **[Richardson Maturity
Model](https://www.martinfowler.com/articles/richardsonMaturityModel.html).**
is a way to score an API according to the constraints of the RESTful
architectural style.

An API with a score of 0 is not deemed RESTful whereas an API with a
score of 3 is a truly RESTful API.

The following table describes the Richardson Maturity Model:

| Level | Name                | Description                                                                                                                                                                                          |
|-------|---------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 0     | Swamp of POX        | Uses HTTP as a transport protocol only without using other HTTP capabilities e.g. SOAP would typically be see as level 0 <BR> (POX = ‘Plain Old XML’)                                                |
| 1     | Resources           | An endpoint per resource type                                                                                                                                                                        |
| 2     | HTTP Verbs          | Use HTTP properties to deal with scalability and faults e.g. HTTP methods, HTTP response codes                                                                                                       |
| 3     | Hypermedia controls | Use link elements in the response to address what may be done with further requests. <BR> This level of compliance is more commonly known as Hypermedia As The Engine Of Application State (HATEOAS) |

> Finastra Open APIs **SHOULD** target level 2: HTTP Verbs

> Finastra Open APIs **MAY** target level 3: Hypermedia Controls

# Payloads and Formats

Finastra APIs should use industry standard open data formats, standards
and protocols before using custom formats.

> APIs **MUST** support JSON format.

## JSON Payloads

JSON, pronounced *jason*, is a lightweight data interchange format; an
overview of JSON can be found here: [www.json.org](www.json.org).

JSON format is described in [RFC
7159](https://tools.ietf.org/html/rfc7159) which provides a high level
description of the syntax and data types.

JSON has replaced XML as the *de facto* standard for data format, and
has broad support across all languages and technology stacks that
Finastra support.

Note that the JSON specification does not dictate the order in which
properties are serialized, hence, the API and/or calling client must not
assume the order of properties within a JSON payload.

> The API `client` **MUST NOT** rely on the ordering of properties or
> values within the JSON payload.

`TimeStamp` MAY be used but is discouraged, as it does not follow the
rule of being human readable. JSON also does not specify the naming
format of the attribute name, whereas it is crucial to have a kind of
homogeneous ways of naming attribute.

> APIs **MUST** use ISO 8601 for `Date` and `DateTime` types within JSON
> payloads

> APIs **SHOULD NOT** use `TimeStamp` types within JSON payloads

> APIs **SHOULD** define a dedicated content type when transferring
> binary as `base64` encoded text

## Other Payloads

A resource may need to add attachments such as images, CSV files etc.

The following example shows how a file can be handled using:

-   the HTTP `multipart/form-data` content type
-   the payload defined as a `formData` parameter using a type of `file`

``` notoggle
"/import": {
      "post": {
        "description": "To import files into the system ",
         "produces": [
          "application/json"
        ],
        "consumes": [
          "multipart/form-data"
        ],
          "parameters": [
            {
              "type" : "string",
              "name" : "File meta data"
              "in" : "formData"
            }
            ,
          {
            "type": "file",
            "description":"File to upload",
            "name": "document",
            "in": "formData",
            "required": true
          }
        ],
```

## XML and Other Standard Formats

XML is a common data format standard and may still be required for
integration.

> APIs **MAY** use XML **if and ONLY if** it follows industry data
> format standards.

> Non-JSON standards such as FPML **SHOULD NOT** be converted to JSON,
> rather use the content-type associated with the standard e.g. XML for
> FpML.

> **MAY** use the **Financial-grade API**
> [FAPI](https://openid.net/wg/fapi/).

The following is a list of existing standards:

-   [ACORD
    P&C](https://www.acord.org/standards-architecture/acord-data-standards/Property_Casualty_Data_Standards)
-   [EDIFACT](http://www.unece.org/cefact/edifact/welcome.html)
-   [FAPI](https://openid.net/wg/fapi/)
-   [FIX](https://www.fixtrading.org/standards/)
-   [FpML](http://www.fpml.org/)
-   [GRLC](https://www.acord.org/standards-architecture/acord-data-standards/Global_Reinsurance_Data_Standards)
-   [HR-XML](https://hropenstandards.org/download-hr-open-standards-landing-page/)
-   [ISO 20022](https://www.iso20022.org/)
-   [L&A](https://www.acord.org/standards-architecture/acord-data-standards/Life_Annuity_Data_Standards)
-   [MDDL](https://web.archive.org/web/20140104144101/http://www.mddl.org:80/mddl/3.0-beta)
-   [OFX](http://www.ofx.net/)
-   [SDMX](https://sdmx.org/?page_id=5008)
-   [SWIFT](https://www.swift.com/standards)
-   [XBRL](https://www.xbrl.org/)

# Responses and Errors

<!-- ## Return Code and Error Management -->

### HTTP Responses

HTTP response status code are defined according to section 6 of
[RFC7231](https://tools.ietf.org/html/rfc7231#section-6).

The following table summarises these response codes:

| Response code | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|---------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **200**       | **OK** - this code is a standard response for successful HTTP requests. The actual response will depend on the request method used. In a GET request, the response will contain an entity corresponding to the requested resource. In a POST request, the response will contain an entity describing or containing the result of the action                                                                                                                                                                                                                                                                               |
| **201**       | **Created** - this code indicates that a request has been fulfilled, resulting in the creation of a new resource                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |
| **204**       | **No content** - this code indicates that the server has successfully fulfilled the request and there is no additional content to send in the response payload body                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| **304**       | **Not Modified** - this code indicates that the resource has not been modified since the version specified by the request headers *If-Modified-Since* or *If-None-Match*. In such case, there is no need to re-transmit the resource since the client still has a previously-downloaded copy. This code was introduced in [RFC 7232](https://tools.ietf.org/html/rfc7232)                                                                                                                                                                                                                                                 |
| **400**       | **Bad Request** - this code indicates that the server cannot or will not process the request due to an apparent client error such as malformed request syntax, size too large, invalid request message framing, or deceptive request routing                                                                                                                                                                                                                                                                                                                                                                              |
| **401**       | **Unauthorized** - this code must be used when there is a problem with the client’s credentials. A 401 error response indicates that the client tried to operate on a protected resource without providing the proper authorization. It may have provided the wrong credentials or none at all                                                                                                                                                                                                                                                                                                                            |
| **403**       | **Forbidden** - this code should be used to forbid access regardless of authorization state. A 403 error response indicates that the client’s request is formed correctly, but the REST API refuses to honor it. A 403 response is not a case of insufficient client credentials; that would be 401 (“Unauthorized”). REST APIs use 403 to enforce application-level permissions. For example, a client may be authorized to interact with some, but not all of a REST API’s resources. If the client attempts to get a resource interaction that is outside of its permitted scope, the REST API should respond with 403 |
| **404**       | **Not Found** - this code must be used when a client’s URI cannot be mapped to a resource                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| **409**       | **Conflict** - this code indicates that a request could not be completed due to a conflict with the current state of the target resource e.g. when a request to update a resource is based on an earlier version of the resource. This status code is used in situations where the user might be able to resolve the conflict and resubmit the request                                                                                                                                                                                                                                                                    |
| **415**       | **Unsupported Media Type** - this code indicates that the origin server is refusing to serve the request because the payload is in a format that is not supported                                                                                                                                                                                                                                                                                                                                                                                                                                                         |
| **500**       | **Internal Server Error** - this code is a generic error message, given when an unexpected condition was encountered and no other specific message is suitable                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| **503**       | **Service Unavailable** - this code indicates that the server is currently unable to handle the request as a result of a temporary condition which will be alleviated after some delay                                                                                                                                                                                                                                                                                                                                                                                                                                    |

**Finastra Responses**

> The following error codes **MUST** be supported:

-   either 200 or 201 or 204 for a successful response and
-   400, 401, 404, 500

> The following error codes **SHOULD** be supported: 304, 403, 409

> Other error codes **MAY** be used, such as: 415, 503

> Finastra APIs **MUST NOT** return 200 (OK) when there is a technical
> or functional error.

## Finastra Error Message Structure

This section includes the following:

-   details of [RFC 7807](https://tools.ietf.org/html/rfc7807) - this is
    the basis of the Finastra error message structure
-   details of Finastra standards relating to error messages - these
    standards must be adopted by Finastra APIs

### RFC 7807

RFC 7807 provides a standard format for returning problem details from
HTTP APIs.

> Finastra APIs **MUST** align with RFC7807

RFC7807 proposes a structure for common pattern of errors whilst also
allowing for extensions. The following text is taken from RFC7807 and
describes the principal fields of the error message structure that :

-   `type` (string) - A URI reference \[RFC3986\] that identifies the
    problem type. This specification encourages that, when dereferenced,
    it provides human-readable documentation for the problem type,
    using, for example, HTML (\[W3C.REC-html5-20141028\]). When this
    member is not present, its value is assumed to be
    `{.notoggle .json}about:blank`.
-   `title` (string) - A short, human-readable summary of the problem
    type. It SHOULD NOT change from occurrence to occurrence of the
    problem, except for purposes of localisation using, for example,
    proactive content negotiation - see \[RFC7231\], Section 3.4).
-   `status` (number) - The HTTP response status code (\[RFC7231\],
    Section 6) generated by the origin server for this occurrence of the
    problem.
-   `details` (string) - A human-readable explanation specific to this
    occurrence of the problem.

### Finastra Error Message Standards

The following are the Finastra Open API error message standards:

-   the main error message **MUST** follow RFC7807 structure and names
-   the use of the RFC7807 `instance` keyword is **NOT** mandated but
    **MAY** be used
-   error messages **MUST NOT** expose details of the technical
    implementation
-   title and status **MUST** be defined in the API; all other fields
    are optional
-   if a field is not used then it **SHOULD NOT** be included in the API
    e.g. `causes` is only required if additional business reasons are
    added
-   `causes` can be used to provide addi"tional detail on an error
    message or to provide a list and sequence of a chain of error
    messages from an end to end system
-   the structure of `causes` is not mandated, however, when `causes` is
    provided it is likely that the client will need to know the name of
    the field in error
-   text capable fields **MAY** support multi-language

The standard Finastra error message structure is as follows:

``` notoggle
{
    "type": "https://api.finastra.com/validation-error",
    "title": "The request could not be processed due to applicable business validation",
    "status": 400,
    "detail": "Ensure that the request is valid"
}
```

If additional details of one or more error messages are needed then an
array of `causes` can be provided, however, the presence and structure
of `causes` is not mandated.

The following example shows how `causes` might be used:

``` notoggle
{
  "type": "https://api.finastra.com/validation-error",
  "title": "The request is invalid",
  "status": 400,
  "detail": "The account does not exist",
  "causes": [
    {
      "title": "The account '1234567890' is dormant",
      "field": "account",
      "fieldValue": "0543123467083"
    }
  ]
}
```

# Naming Conventions

These are the default naming conventions for REST.

## Resources

A key principle of REST involves separating your API into logical
resources that are aligned with the business domain.

The business domain resources are manipulated using HTTP methods as
required by the business.

A Finastra business domain resource must:

-   be defined consistently within an API as a **plural noun**
-   be a concrete concept e.g. `accounts`
-   have a unique identifier and URI

This aligns with common industry convention.

For instance, when defining an API for an `Account` resource the
convention is to use `accounts` rather than `account` for all HTTP
operations defined within the API.

Example:

``` notoggle
paths:
  /accounts:
    "GET"   < return a list
    "POST"  > create a single entity
```

### Composite and Path Naming

Resources should be addressable via a URI which implies that Resources
should be DNS compatible with
[RFC1035](https://tools.ietf.org/html/rfc1035).

To align with RFC1035, resources within Finastra APIs must adhere to the
following:

> Resource **MUST** not contain characters, such as *space*, that
> require URL encoding.

> Resource **MUST** use **kebab case** where a hyphen ( - ) is the
> optional separator e.g. a **Big Car** resource should be mapped to
> `/big-car` rather than `/BigCar` or `/big_car` or `/Big-Car`

### URI for Dedicated Action

Use cases may arise where the HTTP method is not adequate to perform an
action on the resource.

In these cases an action, named a **resource controller**, **MAY** be
used against a resource, for example:

-   `POST /accounts/{accountId}/transactions/{transid}/confirm`, would
    confirm an existing transaction but the same result can be achieved
    with:

-   `POST /accounts/{accountId}/transactions/{transid}/confirmation`,
    that would create a confirmation.

> Note that the **resource controller** approach is **NOT** recommended
> and alternative designs should be explored

Finastra API Standards:

> Use standard HTTP Verbs for CRUD operations against a resource

> Queries for an individual resource and collection of resources
> **SHOULD** use a plural noun as the resource name

> Actions and Commands **MAY** be use a present tense verb

### Resource Identifier

> Resource identifiers **SHOULD** be defined within the path and not as
> a the path parameter

For example:

-   `GET /accounts/{identifier}` MUST be used to identify the resource
    to be retrieved

-   `GET /accounts?id={identifier}` can be used to retrieve a resource
    but only as a search filter, hence, it should be avoided

> **SHOULD** not expose implementation details inside the
> `key/identifier` path e.g. incremental identifiers may provide
> guessable identifiers

> **SHOULD NOT** use UUIDs unless necessary as they are more difficult
> to identify

In some cases the resource identifier represents the concatenation of
one or more fields of the resource. In these cases a composite key
**MUST** use hyphen as a separator.

For example -

``` notoggle
{
    key1 : ABC
    key2 : 123
    key3 : A
    ...
}
```

is addressable as `GET /resource/ABC-123-A`.

> It is recommended that an alternative approach is adopted if possible
> to allow extensibility.

### Child Resources

Child resources \*MAY\*\* be used when the child resource has a life
cycle related to the parent resource, for example:

``` notoggle
/accounts/{id}/transactions
```

The number of nested child resources **MUST** be limited to a maximum of
three to reduce the complexity of the API

## Resource Payload

The fields within JSON payloads must be named in a clear, concise and
unambiguous manner.

> Attributes and parameters **MUST** be in lower `camelCase` with
> optional hyphens

For example:

-   good: `inputDate`
-   invalid: `InputDate`, `Input_Date`

For booleans, the field must be unambiguous hence `isValid` is preferred
to `isInvalid`.

> Child property names **SHOULD NOT** repeat the parent name

For example:

``` notoggle
// Do NOT repeat "account" in child properties 

// **CORRECT**
{
    "account": {
        "id": 0543123467083,
        "label": "Savings account"
    }
}
// **INCORRECT**
{
    "account": {
        "accoundId": 0543123467083,
        "accountLabel": "Savings account"
    }
}
```

> Path parameters **MUST** be in lower `camelCase` with optional hyphens

For example:

``` notoggle
GET /accounts?includeClosed=true
```

## Abbreviations

> Abbreviations **SHOULD** be avoided

To avoid ambiguity, abbreviations e.g. txn rather than transaction
should be avoided in API definitions. Commonly used well known
abbreviations are permitted e.g. URL, IBAN, SEPA etc.

## Enumerations

Enumerations are used to define a closed set of allowed field values
e.g. the following shows a set of possible values for an item’s status:
`PENDING`, `APPROVED`, `COMPLETE`.

Enumeration values:

-   **MUST NOT** be removed from a released enumeration
-   **SHOULD NOT** be added to an enumeration without informing API
    clients
-   **SHOULD** only use characters: a-z, A-Z, hypen
-   **MUST NOT** include spaces or special characters e.g. underscores
-   **MAY** use hyphens to separate words

# Versioning

<!-- ## Versioning -->

Finastra APIs represent contracts with clients and the introduction of
breaking changes always results in a new version of an API.

The client is not informed about the changes that do not break the
contract e.g. minor documentation changes

The following versioning standards apply to Finastra APIs:

> Finastra APIs **MUST** version each API endpoint because the version
> clearly indicates to clients when an API introduced breaking changes

> Finastra APIs **MUST** be designed to be backwards compatible as far
> as possible

> Finastra APIs **MUST** change the version when breaking changes are
> introduced to the API

> APIs **SHOULD** differentiate finished API definitions from work in
> progress

> Finastra APIs **MUST NOT** be obsoleted when in use by customers; a
> published deprecation and obsoletion with appropriate notice periods
> must be provided

## Backward compatibility

This sections details backward compatibility considerations for Finastra
APIs.

The expectation of clients is that a client calling a server API at
version 1 should be able to call the API at version 1.1 without
problems, for example, the same request payload should return ‘at least’
the same response payload.

Similarly there are expectations that the server has of the client:

-   the server assumes that the client is “fault tolerant in reception”
    especially with meta data fields e.g. `Unknown` fields should be
    ignored and `Unknown` values should be considered an error

-   behavioural change within the API is not necessarily considered a
    breaking change e.g. a ‘rate limiting’ error should not be
    considered a breaking change because the client is assumed to be
    capable of handling any 4XX or 5XX errors

### Non-Breaking Changes

The following is a list of non-breaking changes that should result in a
backwardly compatible API:

**Adding an endpoint to an existing API, extending resource end point,
adding a verb**

Assuming the binding does not introduce any ambiguities, making the
server respond to a URL it previously would have rejected is safe. This
is the case for adding a new end point as well as completing the
coverage of an existing resource.

**Extending the range of a request value**

If a new optional parameter is introduced in any part of the `query`,
`path` or `body`, of a request, then the change is deemed to be a
non-breaking change.

If a new mandatory parameter with a default value is introduced then
this would also be deemed a non-breaking change.

**Adding a field to a response message (not a resource )**

The server assumes that the client is fault tolerant, hence, adding a
new field to a response is deemed to be a non-breaking change

**Adding a value to an enum**

An enum that is only used in a request message can be expanded to
include new elements since the adoption of the new enum value is driven
by the client.

An enum that is used in a response message can be expanded to include
new elements. In this case the server **MAY** assume that clients should
handle enum values they’re not aware of, however, API designers should
be aware that writing applications to correctly handle new enum elements
may be difficult and API designers should document expected client
behavior when encountering an unknown enum value.

**Behavioural change**

Behavioural change within the API is not necessarily considered a
breaking change e.g. a ‘rate limiting’ error should not be considered a
breaking change because the client is assumed to be capable of handling
any 4XX or 5XX errors

**Changing an HTTP binding**

When an API endpoint url is modified e.g. from PUT to PATCH then the
change should not be implemented as delete PUT and add Patch, rather the
change can be implemented as a new PATCH endpoint which provides a
non-breaking change, otherwise the change is a breaking change.

### Breaking Changes

The following is a list of breaking changes that would result in a
backwardly INcompatible API:

**Removing or renaming a path, field, enum value**

Client code that refers to API elements that have been renamed or
removed will break on calling the API, hence, renaming or removing API
elements **MUST** result in a major client version change.

**Changing the type of a field**

When a field type is changed this will most likely break the client code
and \*libraries and therefore **MUST** result in a major version change.

## REST versioning

This section describes the versions used in Finastra APIs and how the
version is made available to clients.

A Finastra API has two versions:

-   the Swagger version held in the `info: section` of a Swagger file -
    this is used to record all changes to the API e.g.:

``` notoggle
info:
      version: 1.0.0
```

-   the client version - this is held in the path parameters section for
    each endpoint.

Note that REST APIs in the industry also use client versions that can be
defined as: query parameters, custom headers or accept headers with
custom types. These different formats **MUST NOT** be used in Finastra
APIs

The following example show how the versions might be used:

-   `/v1/accounts` - Swagger info.version: v1.0.0 for the initial
    version
-   `/v1/accounts` - Swagger info.version: v1.0.1 is a minor change to
    modify documentation
-   `/v1/accounts` - Swagger info.version: v1.1.0 is a minor change to
    add new functionality such as an optional field or new endpoints
-   `**/v2/**accounts` - Swagger info.version: v2.0.0 is a major change
    such as the introduction of a breaking change - this modifies the
    url so must be published to the client

The following standards apply within Finastra:

> API operations **MUST** declare versions via path parameter
> e.g. `https://{host}/:version/{resource}`

> API client versions against API endpoints **MUST** be incremented for
> breaking changes

<!--
  NOTE: Azure APIM handles the API versioning using the Finastra `APIConfig.json` file, hence the client version is NOT included in the Swagger file
-->

> The API consumer **MUST** be able to clearly and consistently
> understand whether the Finastra API they are using is a `beta`,
> `production` or `deprecated` - this is defined using the
> `x-finastra-maturity-level` attribute as one of “BETA”, “GA” or
> “DEPRECATED”

Notes:

-   The version parameter in Finastra APIs is an `integer` prefixed with
    the letter `v`
-   The version parameter will show up in telemetry
-   A single resource can have many URLs due to versioning
-   Using path parameters may impact memory requirements for proxy-based
    caches

<!-- # Security -->
<!-- ```{r child="md/api-tokenisation.Rmd"} -->
<!-- ``` -->

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

# Bulk Operations

Bulk operations can be used as an approach to limit the number of client
API calls to a server by combining multiple client requests within a
single API endpoint. This approach may help to alleviate performance
issues.

An example of a bulk operation API is when a corporate wishes to upload
a collection of payments in a single API call rather than multiple
calls.

It should be noted that there are drawbacks to providing bulk operations
primarily with synchronicity, atomicity and error handling, for example,
when a single item in a collection fails does the entire collection need
to be rolled back or compensated? As a result the provision of bulk
operations should be avoided with Finastra APIs as far as possible.

> Finastra APIs **SHOULD NOT** provide bulk operations unless necessary;
> there are alternative designs that can be used to avoid bulk
> operations

If bulk operations are required then the following design concerns
should be fully considered:

-   Long Running Processes - can an alternative approach be used to
    implement bulk operations - see the *Long Running Processes* section
    for further details
-   Bulk DELETE - it is strongly advised that DELETE operations are NOT
    provided as bulk operations and that if necessary the operation
    should flag items for deletion
-   Bulk POST - POST bulk operations will not be capable of returning an
    HTTP status code of 201, hence, 200 should be returned
-   Resource Names - bulk operations should clearly identify the
    operation as bulk by using a resource name such as `bulk-users`
-   Synchronicity - a bulk operation is unlikely by its nature to be
    capable of a synchronous response, hence, it may be better to
    provide the bulk operation as a `POST queue-request` endpoint
    coupled with a `GET queue-request/{jobId}` which would return the
    status of the job
-   Atomicity - a synchronous bulk operation should be atomic so when
    the call is successful all items succeed, and when the call fails
    all items are rolled back
-   Error handling - for non-synchronous requests recovery processes
    which may need to be manual may need to be implemented e.g. if item
    723 of 1299 fails to be applied then it may be better to manually
    fix item 723 rather than reject the entire batch
-   Concurrency - concurrent requests for individual and bulk operations
    should be supported by the server

# Server Events and Long Running Tasks

This section describes the following REST API approaches to handling
published server events and long running tasks:

1.  [Long Polling](https://www.ably.io/concepts/long-polling)
2.  [Webhooks](https://realtimeapi.io/hub/webhooks/)

Notes: - Webhooks are supported in OAS v3.0 - WebSockets is an
alternative technology that can be used to support events and long
runnning tasks, however, WebSockets is outside the scope of this Guide
as it is not REST API based. For some use cases WebSockets may be the
most appropriate technology and should be explored further as required

## Long-Polling

Long-polling involves a client subscribing to events or requesting the
status of a long running task from a server through a mechanism similar
to polling but with a significant difference being that the client does
not have a ‘polling retry’ timer, rather then client specifies a timeout
period and the server holds a long lived connection.

The server principle is the long running task returns an id and the
response status code
`202 "The request has been accepted for processing, but the processing has not been completed"`.

Then, the client pulls the new created resource and get a `202` code
until the action is completed.

**The workflow**

1.  Request (create task)

``` notoggle
POST /complexReport HTTP/1.1
{...}
```

1.  Response (return task Id)

Return the 202 ‘Accepted’ HTTP response code.

``` notoggle
HTTP/1.1 202 Accepted
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
```

1.  Request (request task status by Id)

``` notoggle
GET /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a HTTP/1.1
Accept: application+json
```

1.  Response (return status or redirect to task return value)

``` notoggle
HTTP/1.1 202 Accepted
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
{
  "status": "..."
}
```

``` notoggle
HTTP/1.1 200 OK
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
{
  "reportData": [ ... ]
}
```

## Webhooks

Webhooks are HTTP callbacks that receive notification messages for
events.

There are typically two types of Webhooks models:

-   **1-to-1 model**
-   **1-to-many model**

**Letter Box Model - 1-to-1**

Whenever a server event is published, the server makes a REST API call
to the one subscribing client’s API pre-defined endpoint .

**Letter Box Model - 1-to-many**

Whenever a server event is published, the server makes a REST API call
to one or more subscribing clients’ API endpoints.

The server provides API endpoints to allow clients to subscribe to
events so that the client can be informed of the events they register
for.

To provide Webhooks the server needs to implement APIs to allow clients
to:

-   register a Webhook
-   cancel a Webhook
-   optionally - list event types and characteristics that are available
    for clients to subscribe to

# Customisation

This section describes customization capabilities that may be provided
with Finastra Open APIs and includes the following sections:

-   Localisation
-   Custom Fields

## Localisation and Internationalization

> The difference between these two topics is out of scope for this
> guide. For details, see [Localisation
> vs. Internationalisation](https://www.w3.org/International/questions/qa-i18n)
> article by W3C.

Localisation and internationalization are expensive to implement
correctly. APIs should be concerned with providing a business capability
to the customer and not with content negotiation. The UI and its
supporting services should be responsible for these concerns.

> APIs **SHOULD NOT** be concerned with either localisation or
> internationalization.

Out of scope for APIs:

-   Localisation
    -   Translations
    -   RTL and other orientations
-   Internationalization
    -   Decimal separators
    -   Currency

In scope for APIs:

-   Date
-   Date-Time
-   Time

> APIs that support either localisation or internationalization **MUST**
> understand the `Accept-Language` header to facilitate content
> negotiation with the client and inform the client of the selection in
> the `Content-Language` header.

The standard HTTP Header `Accept-Language` informs the service what
languages the client is able to understand, and what locale variant is
preferred. Using content negotiation, the service selects one of the
proposed language and locales, and confirms the selection within the
response header `Content-Language`. Browsers use this header to align
their UI.

The `Accept-Language` header format contains two items: language,
culture.

In the following example, the French language is identified with the two
letter code `FR`; the Swiss culture/locale, which is optional in this
format, is identified by the two-letter code `ch`.

Example:

``` notoggle
FR-ch
```

For more details, see [HTTP 1.1 RFC:
section-5.3.5](https://tools.ietf.org/html/rfc7231#section-5.3.5).

## Custom Fields

Many Finastra products provide the capability for clients to define
additional fields to business objects - this capability is typically
provided by back office systems as an ‘out of the box’ feature developed
by the product R&D team e.g. to allow clients to add their own custom
fields to SME customers for head office reporting.

The following lists typical terms used to describe these additional
fields:

-   custom fields - this is the term used in this log
-   extension fields
-   additional information fields
-   user defined fields

Custom fields are typically used within back office integration projects
where the additional data is captured by front office applications and
passed to the back office via product APIs.

Open APIs are expected to define a fixed set of well known fields that
the FinTech developer community can clearly understand leading to
increased API adoption.

> It is NOT expected that Open APIs will provide support for custom
> fields because of the increased complexity to the developer and
> support overheads for Finastra, however, there may be use cases for
> specific products where custom fields are a necessary requirement for
> an API.

**Finastra Standards**

It is strongly recommended that user defined fields are NOT supported in
Open APIs.

Where user defined fields must be supported due to business requirements
then:

-   user defined fields should be identified using option 1 in the
    diagram above i.e. using the `additionalProperties` attribute with a
    named field of custom
-   a separate operation with the Swagger API should define the meta
    data associated with the user defined fields, for example:
    `GET /extensions?type=customer` would provide the client developer
    with details of the necessary fields including e.g.: name,
    description, data type, validation rules etc.

``` notoggle
account:
  type: object
  required:
    - accountId
  properties:
    accountId:
      type: string
      example: 0543123467083
 
    # named additionalProperties
    custom-fields:
      type: object
      additionalProperties:
        type: string
        example: custom-field-value
```

This YAML would generate the following JSON body in the Swagger editor

where: `"additionalPropN": "custom-field-value"` represents a set of
specific `key:value` pairs as defined by an associated meta data API:

``` notoggle
{
  "account": {
    "accountId": "0543123467083",
 
     "custom-fields": {
      "additionalProp1": "custom-field-value",
      "additionalProp2": "custom-field-value",
      "additionalProp3": "custom-field-value"
    }
  }
}
```

> Custom fields **SHOULD NOT** be a default option, rather, they should
> be used in exceptional circumstances only

> Open APIs **SHOULD** use the `additionalProperties` attribute with a
> named field when providing support for custom fields

# Concurrency

In the context of REST APIs, concurrency refers to the simultaneous
occurrence of multiple requests.

In systems that use optimistic locking, GET, POST and DELETE methods do
not typically result in concurrency problems. However, data consistency
problems might occur when updating resources using PUT or PATCH methods,
when appropriate concurrency designs are not implemented.

For example:

-   Two users have performed a GET against a resource, such as
    `GET /accounts/0543123467083` and they both receive a payload of A.
-   The first user modifies the payload A and requests an update with a
    new payload B: `PUT /accounts/0543123467083 {B}`.
-   The second user modifies the payload A and requests an update with a
    new payload C: `PUT /accounts/0543123467083 {C}`.
-   If both users request the PUT method at approximately the same time
    then the end result of the updated account’s payload will be either
    B or C.
-   In this scenario, the updates made by one of the users is lost.
-   The solution to this problem is to ensure that one of the user is
    returned an error message stating that their update cannot be made.

This scenario can be avoided when using optimistic locking by ensuring
that update operations are designed to occur only when the original
payload (payload A in the example) matches the resource current payload.
The implication of this approach in the example is that one of the users
will be issued with an error message stating that the update cannot be
made because the data has been modified from its state at the time of
the GET by a separate unrelated update.

With REST APIs the solution is typically implemented using the following
HTTP headers:

-   `ETag` - this HTTP header field is a string - a tag, that is
    returned when a GET is requested against a resource. The `ETag`
    header contains a representation of the resource at the time the GET
    occurred and at its simplest. The tag can be a version number or a
    timestamp (although this is best avoided in a distributed
    environment) but it is more likely to represent a hash of the
    resource’s payload fields

-   `If-Match` - this HTTP header field is a string that is passed when
    a PUT is requested against a resource. The `If-Match` header
    contains the `ETag` header previously obtained from the GET request.
    The server will ensure that the PUT request is only performed if the
    value in the `If-Match` header matches the derived `ETag` value of
    the current state of the resource. If the update request cannot be
    performed due to a mismatch then a 412 HTTP status code must be
    returned. If the `If-Match` header is not in the PUT request then a
    *428* HTTP status code must be returned

<span class="text-title">**Finastra API Standards**</span>

The implementation of concurrency using `ETag` and `If-Match` headers is
not mandatory for all APIs since it imposes strict rules on the client
and server, however, full consideration of the implications of NOT
implementing concurrency must be considered especially when backend
servers support updates through a variety of different channels in
addition to REST APIs.

Therefore:

> Concurrency **MUST** be considered for all API requests

> Concurrency **SHOULD** be supported for PUT and PATCH requests

If concurrency is implemented then the following Finastra standards
apply:

> Finastra APIs supporting concurrency with optimistic locking **MUST**
> define `ETag` and `If-Match` headers on associated GET and PUT
> operations

> Finastra APIs supporting concurrency with optimistic locking **MUST**
> return 412 status code if the `If-Match` header on a PUT request does
> not match the derived ETag of the current state of the resource

> Finastra APIs supporting concurrency with optimistic locking **MUST**
> return 428 status code if the `If-Match` header is not included on a
> PUT request

<span class="text-title">**Sample API Code**</span>

The following code snippets show sample OAS2 definitions of GET and PUT
operations within a Finastra API that supports concurrency:

``` notoggle
 get:
...
   responses:
     200:
       schema:
         $ref: '#/definitions/test’
       headers:
         ETag:
           description: Used to cache key for future requests
           type: string
```

``` notoggle
 put:
   operationId: putEntry
   parameters:
   - name: If-Match
     in: header
     description: For updates this field needs to be present and contain an Etag value
     type: string
     required: true
...
   responses:
...
     412:
       description: Precondition Failed - version provided in the If-Match header is invalid
       schema:
         $ref: '#/definitions/ErrorDescription’
     428:
       description: Precondition Required - version must be provided in the If-Match header
       schema:
         $ref: '#/definitions/ErrorDescription'
```

References:

-   [MDN web docs:
    ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)
-   [RFC 7232, section 2.3:
    ETag](http://tools.ietf.org/html/7232#section-2.3)

# Idempotency

In the context of REST APIs idempotency refers to the capability of a
system to safely retry identical requests without performing the same
operation twice. That way making multiple identical requests has the
same effect as making a single request. This capability is necessary
when HTTP is used as a transport mechanism because network failures will
occur and responses will be lost, albeit infrequently. Hence, the
ability to retry requests safely adds resiliency to the system. For
example, if the response to a payment request is not received by a
client then if the server has implemented idempotency the client can
safely retry the request without making the payment twice.

The following indicates which HTTP methods are idempotent when RESTful
API principles have been followed:

-   GET, HEAD and OPTIONS methods are idempotent since multiple,
    identical requests will behave the same
-   DELETE can pragmatically be seen as idempotent since the server
    state is unchanged following identical requests
-   POST is not idempotent since repeated requests will create a new
    resource and return a unique resource identifier for each request

Finastra APIs should be designed to ensure that POST requests are
idempotent - this is done by the client specifying the following HTTP
header when making POST requests:

-   `Idempotency-Key` - this HTTP header field is a string that contains
    a unique idempotency key, ideally a v4 UUID. This key is created by
    the client and the server uses the key to recognize subsequent
    retries of the same request.

Where idempotency is implemented, the backend server must save the
status code and body of the response against the idempotency key,
regardless of whether it succeeded or failed. Subsequent requests with
the same key must then return this stored repsonse. Note that the
response is only stored if the backend started processing the request as
part of a transaction.

The backend server must also consider the following implementation
details:

-   The value of the `Idempotency-Key` can be the same value as an
    existing payload field as long as the payload field is guaranteed to
    be unique.
-   Publish the longevity of idempotency keys - keys are typically
    purged after 24 hours.
-   To ensure that the client is not generating duplicated idempotency
    keys, it may be beneficial to compare incoming request payloads with
    stored request payloads and return an error if they differ.
-   The `Idempotency-Key` header is not the same as the `X-Request-Id`
    header.

The client must also consider the following implementation details:

-   The client must specify a unique value for `Idempotency-Key`.
-   The client is expected to implement a retry strategy if a **5xx**
    message is received or if a response is not receieved

<span class="text-title">**Finastra API Standards**</span>

The implementation of idempotency using `Idempotency-Key` is not
mandatory for all APIs since it imposes strict rules on the client and
server, however, full consideration of the implications of NOT
implementing idempotency must be considered, hence:

> Idempotency MUST be considered for all API requests

> Finastra APIs **SHOULD** support idempotency for POST methods

> Finastra APIs supporting idempotency **MUST** define an
> \`Idempotency-Key header on associated POST operations

<span class="text-title">**Sample API Code**</span>

The following code snippet shows a sample OAS2 definition of a POST
operation within a Finastra API that supports idempotency:

``` notoggle
 post:
    parameters:
      - name: Idempotency-Key
         in: header
         description: Idempotency key will be valid for 24 hours
         type: string
         required: true
```

<!--chapter:end:part-api-design-technical.Rmd-->

# (PART\*) Appendix

# Glossary

This section describes the terms used within this guide with emphasis on
their meaning within Finastra.

**API**

An
[API](https://en.wikipedia.org/wiki/Application_programming_interface)
is a set of definitions and communication protocols that allow
communication between components of a system.

Finastra products publish many different types of API, such as JMS APIs,
SOAP APIs, File APIs, Websockets APIs etc., however, the focus of this
guide is Open APIs that use HTTP as a communication protocol and adhere
to RESTful concepts.

**CRUD**

CRUD refers to **C**reate, **R**ead, **U**pdate, **D**elete and is used
in the context of operations that can be performed against a resource or
business object.

The CRUD operations are comparable to the following REST API operations:
POST, GET, PUT, and DELETE.

Many Finastra products publish CRUD API operations to allow clients to
capture and retrieve data stored in the product data store.

CRUD operations may not be relevant to products that adopt a [Command
and Query Responsibility Sgeregation
(CQRS)](https://martinfowler.com/bliki/CQRS.html) pattern.

**HAL**

[HAL](https://en.wikipedia.org/wiki/Hypertext_Application_Language)
refers to Hypertext Application Language and it is a standard convention
for defining links to external resources within JSON or XML code, for
example, if a client requests a resource such as a loan then the
response might return a set of links in HAL format specifying how to
obtain additional details of the resource.

**HATEOS**

[HATEOS](https://en.wikipedia.org/wiki/HATEOAS) refers to Hypermedia as
the Engine of Application State which is a style whereby API responses
provide links that are related to the API requested by a client.

For example, a HATEOS-driven site provides information to clients that
allow them to navigate a site’s resources dynamically by including
hypermedia links with the responses.

**JSON**

JSON refers to [JavaScript Object Notation](https://www.json.org/) which
is a lightweight syntax that can be used to define business objects. It
is seen as a more lightweight version of XML. JSON (or YAML) can be used
to define Finastra Open APIs aligned with the OpenAPI specification.

**Open API** (with space)

An [Open API](https://en.wikipedia.org/wiki/Open_API) refers to an API
that is exposed to developers by a platform such as
[fusionfabric.cloud](https://www.fusionfabric.cloud/).

When an organisation publishes a set of Open APIs they will typically
have the following attributes:
[Useful](#FinastraOpenAPIs-Principles-Useful) [Keep it
Simple](#FinastraOpenAPIs-Principles-KeepitSimple) [Easy to
Use](#FinastraOpenAPIs-Principles-EasytoUse)
[Consistent](#FinastraOpenAPIs-Principles-Consistent) [Backward
Compatible](#FinastraOpenAPIs-Principles-BackwardCompatible)

**OpenAPI** (without space)

An [OpenAPI](https://en.wikipedia.org/wiki/OpenAPI_Specification) refers
to an API that adheres to the [OpenAPI
Specification](https://www.openapis.org/) standard, or Swagger
Specification standard.

These OpenAPI standards describe the structure and syntax of the
document, or code, that is used to describe RESTful APIs.

Finastra APIs are compliant with [OAS
2](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)
or [OAS
3](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)
where OAS is an abbreviation for ‘OpenAPI Specification’.

**Operation** **(or** **Endpoint)**

The operations, or endpoints, define the exposed capabilities of the API
as operations against resources, such as:

``` notoggle
*   GET /trades : to list all trades
*   POST /trades : to create a new trade
*   GET /trades/{id} : to read a trade
```

In this example, there are 3 endpoints, 2 operations named GET and POST,
and 1 resource named trades

**Partner API** **(or Private API or Product API)**

A Partner API refers to an API that is exposed to partners primarily for
the purposes of integrating systems.

Partner APIs are also referred to as Private APIs or Product APIs.

Typically a Partner API will be coarser grained than an Open API since
it is targeted at more generic integration use cases that typically
provide a large set of business object fields as the API payload .

Partner APIs are not part of Finastra’s Open API strategy.

Partner APIs defined as a Swagger file are subject to the same Swagger
Standards detailed in this guide.

**Resource**

A resource in a REST API is a business object associated with the
business domain, such as trades, loans data or function.

**REST**

REST is short for [Representational State
Transfer](https://en.wikipedia.org/wiki/Representational_state_transfer)
and is an architectural concept for the provision of web services.

**REST API**

REST APIs are APIs that use HTTP protocol and adhere to RESTful
principles.

**Swagger file**

A Swagger file is a description, in JSON or YAML format, of a REST API
and its operations. For example an API might be named *Micro-Loans* and
have operations such as *Create Micro-Loan*, *Update Micro-Loan*,
*Delete Micro-Loan*.

Swagger file syntax aligns with the [OpenAPI
Specification](https://www.openapis.org/) which is available at [OAS
2](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)
or [OAS
3](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/3.0.2.md)
where OAS is an abbreviation for ‘OpenAPI Specification’.

**YAML**

[YAML](https://yaml.org/) [](https://yaml.org/spec/1.2/spec.html)refers
to “YAML Ain’t Markup Language”. YAML is a human-readable,
space-sensitive syntax. YAML (or JSON) can be used to define Finastra
Open APIs aligned with the OpenAPI specification.

# Finastra Standards Summary

This section contains a summary of the Finastra Open API standards and
is intended a quick reference guide to the standards.

The standards that have headings preceded by “Swagger” are specific
Swagger standards related to the API definitions.

## Conventions

The key words “MUST”, “MUST NOT”, “REQUIRED”, “SHALL”, “SHALL NOT”,
“SHOULD”, “SHOULD NOT”, “RECOMMENDED”, “MAY”, and “OPTIONAL” in Finastra
standards are to be interpreted as described in [RFC
2119](https://www.ietf.org/rfc/rfc2119.txt)

<!-- # See also https://almtools/confluence/display/ARPP/Style+Guide+-+Overview  -->

## Principles

-   [Useful](#FinastraOpenAPIs-Principles-Useful)
-   [Keep it Simple](#FinastraOpenAPIs-Principles-KeepitSimple)
-   [Easy to Use](#FinastraOpenAPIs-Principles-EasytoUse)
-   [Consistent](#FinastraOpenAPIs-Principles-Consistent)
-   [Backward
    Compatible](#FinastraOpenAPIs-Principles-BackwardCompatible)

## General

-   **MUST** adhere to Finastra API Principles
-   **MUST** adhere to the Swagger/OpenAPI specification - OAS2
-   **MUST** follow Swagger guidelines
-   **MUST** follow Finastra API Concepts Guide
-   **MUST** follow Finastra Open API Style Guide
-   **MUST** support HTTPS for client requests and responses
-   **MUST** be well documented so that, as far as possible, additional
    documentation beyond the Open API is not required
-   **MUST** write APIs in English (US)
-   **MUST** follow the Finastra deprecation and obsoletion policy when
    decommissioning APIs
-   **SHOULD NOT** convert industry standard formats to Swagger
    e.g. ISO20022, FpML, etc.
-   **MUST NOT** break backward compatibility without due consideration
    and notifications
-   **MUST** use absolute URIs rather than relative URIs
-   **MUST** target REST maturity level 2
-   **MAY** target REST maturity level 3 (HATEOAS)
-   **SHOULD NOT** use XML within Finastra Open APIs
-   **SHOULD** adopt an API First strategy and API as a Product strategy
-   **MAY** define Open APIs using OpenAPI 3.0, however, approval MUST
    be obtained
-   **SHOULD** be designed so that they include as many validation rules
    as possible
-   **SHOULD** be backward compatible as far as possible
-   **SHOULD NOT** enforce HTTP 2
-   **SHOULD NOT** include sensitive data in responses
-   **SHOULD NOT** support either localisation or internationalisation
    unless necessary but **MUST** use the `Accept-Language` header *if*
    supporting either localisation or internationalisation

## Names

-   **MUST** be meaningful by obviously declaring the field’s purpose; a
    simple guideline is to ensure that Google shows relevant links on
    the first returned page
-   **MUST** align with the accepted business domain terminology
-   **MUST** adhere to the Finastra Open API data dictionary where
    possible e.g. `country`; where further clarity is required a prefix
    can be used e.g. `taxCountry`
-   **MUST** only use commonly accepted abbreviations
-   **MUST NOT** introduce new terminology where existing equivalents
    are available
-   **MUST NOT** be technical in nature because Open APIs target
    business rather than technical domains
-   **MUST NOT** use terms that relate to a specific product e.g. use
    the agnostic term `region` rather than `processingArea`
-   **MUST NOT** use terms that relate to a specific geography e.g. use
    the agnostic term `alternateAccountId` rather than `micr`
-   **MUST NOT** use terms that relate to a specific channel

## HTTP Methods

-   **MUST** use HTTP methods correctly by following Finastra guidelines
-   **MUST** support idempotency for POST, PATCH, PUT
-   **SHOULD** document any filtering that is performed server side
    e.g. due to access rights

## HTTP Headers

-   **MAY** use standard HTTP headers
-   **MUST** use Train-Case for HTTP header Fields
-   **MUST** use content headers correctly
-   **MAY**: use Content-Location Header
-   **SHOULD**: use Location Header instead of Content-Location Header
-   **MAY**: use ETag Together With If-Match/If-None-Match Header

## HTTP Custom Headers

-   **MUST** use only Finastra approved custom headers
-   **MUST** ensure that Finastra custom headers are propagated end to
    end
-   **MUST** ensure that Finastra custom headers are in `Train-Case`
-   **SHOULD** use X-Request-ID to allow tracking of end to end requests
    and responses

## Swagger info - API Information

-   **MUST** reflect the business domain and purpose

-   **MUST** be concise and relevant

-   **MUST NOT** contain additional images

-   **MUST** contain an Overview section

-   **MUST** contain an Usage section

-   **MUST** ensure the `terms of service` is blank

-   **MUST** ensure the `contact` is blank

-   **MUST** ensure the `license` is blank

-   **SHOULD** have a version entered in `n.n.n`
    [semver](https://semver.org/spec/v2.0.0.html) format to reflect the
    version of the Open API document

-   **MUST** publish a new public version of your API when introducing
    breaking changes

-   **MUST** use a version number beginning with 0. to differentiate
    finished API definitions from work in progress

-   **MUST** contain Finastra specific fields (named with the prefix
    `x-finastra-`)to publish the API to the Finastra Developer Portal

-   `x-finastra-category`
    `x-finastra-subcategory`
    `x-finastra-short-description`
    `x-finastra-tags`
    `x-finastra-channel-type` - one of: DIGITAL - B2B - B2E
    `x-finastra-maturity-level` one of: BETA - GA - DEPRECATED
    `x-finastra-audience` - one of: PUBLIC - INTERNAL - RESTRICTED

## Swagger host - Serving the API

-   **MUST** use prescribed `host:` and `basePath:` values
-   **MUST** support HTTPS for client requests and responses -
    [RFC2660](https://tools.ietf.org/html/rfc2660) and
    [RFC2818](https://tools.ietf.org/html/rfc2818)
-   **MUST NOT** support HTTP

## Swagger security - Security

-   **MUST** secure endpoints with oAuth2
-   **MUST** use a oAuth2 flow of `application` for B2B APIs
-   **MUST** use a oAuth2 flow of `accessCode` for B2C (DIGITAL) APIs
-   **MUST** provide scopes relevant to the API, resource and operation
-   **MUST** define scopes based on the name of the application,
    resource and the operation
-   **MUST** define scopes in the security definitions section of the
    Swagger document

## Swagger paths - Operations and Resources

-   **SHOULD** name resources as plural
-   **MUST** define *useful* resources - see the \[Finastra Open API
    Principles
-   **MUST** use Finastra domain and business naming convention
-   **MUST** obtain approval from the Finastra Open API team if
    introducing new resource names
-   **MUST** use Finastra common business object names where available
-   **MUST** limit the number of resources by ensuring that all new
    resources are Finastra approved
-   **MUST** name resources with nouns
-   **MUST** be consistent in usage of plural and singular for a
    specific resource within an API
-   **MUST** name resources using the characters a-z, 1-9 and hyphens
    e.g.`/shipment-orders`
-   **MUST NOT** use characters that require URL encoding e.g. spaces
-   **MUST NOT** use an underscore character
-   **MUST** use lowercase separate words with hyphens for all path
    segments e.g. `/shipment-orders/{shipment-order-id}`
-   **MUST** identify the resource within the path, and not in the path
    parameter
-   **MUST NOT** use verbs or actions in the path - use resources
-   **MUST** use plural for resource names that can return collections

## Swagger parameters - Parameters

-   **MUST** define parameters in lower `camelCase` restricted to
    characters a-z and 0-9
-   **MUST NOT** include the names of parent fields in names of the
    child fields e.g.{account {id, type}} NOT {account {accountId,
    accountType}}
-   **MUST** define the resource’s unique identifier with `{id}`
-   **MUST** use `$refs` for common parameters
-   **MUST** list parameters in a specific order: required, followed by
    optional
-   **SHOULD** ensure that enumerations are lowercase a-z and/or upper
    case A-Z with hyphens are permitted to separate words
-   **MUST** ensure that date and time properties adhere to ISO 8601
-   **MUST** ensure that time duration and intervals adhere to ISO 8601
-   **MUST** adhere to standards for common business objects
    e.g. country - ISO 3166, currency - ISO 4217, amounts, etc.
-   **MUST** ensure that array names are plural
-   **MUST NOT** define binary content as string
-   **MUST** use well-defined parameter types
-   **MUST** add new optional parameters to the *end* of the parameter
    list
-   **MUST** ensure that `required` properties are not defined in the
    schema of an optional body parameter
-   **MUST** ensure that `number` and `integer types` have an associated
    `format`
-   **MUST** specify relevant defaults where applicable
-   **MUST NOT** specify a default for a required parameter
-   **SHOULD** use the standard media type name `application/json`
-   **SHOULD** avoid reserved words
-   **SHOULD NOT** use `TimeStamp` datatype
-   **SHOULD** define a dedicated content type when transferring binary
    as `base64` encoded text
-   **SHOULD** only use UUIDs where necessary
-   **SHOULD** use empty string to remove a field’s value
-   **SHOULD NOT** use `allowEmptyValue`

## Swagger responses

-   **MUST** specify standard HTTP status codes in responses
-   **MUST** specify 400, 401, 404, 500 and one of 200, 201, 204 HTTP
    status codes in responses
-   **SHOULD** specify 304, 403, 409 HTTP status codes in responses
-   **MAY** specify other HTTP status codes in responses e.g. 429 for
    rate limiting
-   **MUST NOT** return HTTP status code 200 (OK) when there is a
    functional error or technical fault
-   **MUST** specify the most relevant HTTP status code
-   **MUST** specify error details in the Finastra error response based
    on [RFC7807](https://tools.ietf.org/html/rfc7807)
-   **MUST** specify clear, concise and useful error responses
-   **MUST** ensure that technical implementation details such as stack
    traces are not exposed in error messages

## Swagger tags

-   **SHOULD** ensure that tags reflect the name of API’s business
    domain
-   **MAY** include a tag description if additional details improve
    understanding of the purpose of the tag

## Swagger externalDocs

**MAY** use the `externalDocs` element to supplement information
provided with the API

## Other - Pagination

-   **MUST** support pagination for lists that might cause server
    overloads
-   **SHOULD** support sorting for APIs with collections and if so,
    **MUST** have a default sort clearly defined
-   **SHOULD** use either Offset based pagination rather than Cursor
    based pagination
-   **SHOULD** use pagination links where applicable

## Other - Performance

-   **MUST** ensure that performance concerns address response times and
    bandwidth
-   **SHOULD** implement rate limiting

# Finastra Open API Dictionary

This section contains the following sections:

-   general guidelines on naming of resources and fields
-   detailed guidelines for specific field types

The content of this section represents Finastra standards and contains
structures that should be used in Finastra Open APIs as far as possible.

## General Guidelines

Finastra Open API field names:

-   must be defined using standard and plain English
-   must be clear and concise
-   must be meaningful by obviously declaring the field’s purpose; a
    simple guideline is to ensure that Google shows relevant links on
    the first returned page
-   must only use the characters a-z, A-Z and 1-9; characters such as
    hyphens and underscores cannot be used
-   must be in camel case
-   must align with the accepted business domain terminology
-   must align with the Finastra Open API Business Domain and Business
    Object naming convention
-   must adhere to the Finastra Open API data dictionary where possible
    e.g. country; where further clarity is required a prefix
-   must NOT introduce new terminology where existing equivalents are
    available
-   must only use commonly accepted abbreviations
-   must NOT be technical in nature because Open APIs target business
    rather than technical domains
-   must NOT use terms that relate to a specific product e.g. use the
    agnostic term region rather than processingArea
-   should NOT use terms that relate to a specific geography e.g. use
    the agnostic term alternateAccountId rather than micr
-   should NOT use terms that relate to a specific channel
-   should NOT include the names of parent fields in names of the child
    fields e.g. taxCountry in favour of customerTaxResidenceCountry

## Specific Examples

The following specific examples of structures that are used within
Finastra APIs are shown in YAML using OAS2.

### A

**Account identification**: A unique identifier is used to identify an
individual account. Finastra APIs **SHOULD** use the account number or
the IBAN as shown below

``` notoggle
AccountId:
  description: The accountId is a unique and unambiguous identification of a bank account
  type: string
  maxLength: 60
  example: 0543123467087

IBAN:
  description: The International Bank Account Number (IBAN) is an internationally agreed system for unique and unambiguous identification of bank accounts across national borders. The format and content of the IBAN can be found in the standard ISO 13616
  type: string
  maxLength: 34
  pattern: '^[A-Z]{2,2}[0-9]{2,2}[a-zA-Z0-9]{1,30}$'
  example: DE89370400440532013000
```

**Address**: Address structures **SHOULD** use the fields in the
structure below as far as possible. Additional fields may be added as
required

``` notoggle
Addresses:
  title: Addresses
  type: array
  description: Addresses.
  items:
    $ref: '#/definitions/Address'
Address:
  title: Address
  description: Information that identifies the location of an address.
  type: object
  required:
    - country
    - type
    - line1
  properties:
    type:
      type: string
      description: The address type. The list below is indicative rather than exhaustive.
      enum:
        - PO-BOX
        - RESIDENTIAL
        - BUSINESS
        - MAIL-TO
        - DELIVER-TO
        - OTHER
      example: BUSINESS
    country:
      description: Country in which the address is located. Adheres to ISO3166 - Alpha 2.
      type: string
      pattern: '^([A-Z]{2,2})$'
      example: US
    line1:
      description: Address line 1.
      type: string
      maxLength: 70
      example: Starbucks Branch
    line2:
      description: Address line 2.
      type: string
      maxLength: 70
      example: 201 Powell Street
    line3:
      description: Address line 3.
      type: string
      maxLength: 70
      example: ''
    line4:
      description: Address line 4.
      type: string
      maxLength: 70
      example: San Francisco
    line5:
      description: Address line 5.
      type: string
      maxLength: 70
      example: CA
    postalCode:
      description: The postal code associated with the address. Alternative names for this field are postcode and ZIP code.
      type: string
      maxLength: 10
      example: 94102
    buildingNumber:
      description: Number that identifies the position of a building on a street.
      type: string
      maxLength: 16
      example: null
```

**Amount**: An amount **MUST** be defined as a combination of a currency
and a number as shown below

``` notoggle
MonetaryAmount:
  type: object
  description: A monetary amount in a given currency supporting an amount format of 18.3
  required:
    - amount
    - currency
  properties: 
    amount:
      type: string
      description: tbc
      pattern: '-?[0-9]{1,18}(\.[0-9]{1,3})?'
      example: '-5877.78'
    currency:
      description: ISO 4217 Alpha 3 currency code
      type: string
      pattern: '[A-Z]{3}'
      example: EUR
```

**Balances**: Finastra Open APIs **SHOULD** adopt this naming convention
when defining account balances within APIs. APIs do NOT need to include
all the balance types, however, APIs with more than one balance MUST
align with the structure and names

``` notoggle
Balance:
  description: Account balances
  type: object
  properties:
    type:
      type: string
      description: |
        The following balance types are supported, however, a given system may choose to support a subset of balances:
        - BOOKED
          Balance including all account items up to the previous business day *including* uncleared items
        - CURRENT
          Balance which includes all account items since the previous business day including uncleared items but excluding reserved amounts
        - CLEARED
          Balance which includes all cleared account items into and out of the account since the previous business day excluding uncleared items and excluding reserved amounts
        - AVAILABLE
          Balance which is the most up to date balance available for withdrawal - *excludes* uncleared items and *includes* account credit line limits such as an overdraft limit
        - FORWARD
          Balance showing the projected account balance for a future business date including all known posted and unposted items up to and including that date
        - RESERVED
          Balance which includes the total amount of all reserved or blocked amounts held against an account
      enum:
        - BOOKED
        - CURRENT
        - CLEARED
        - AVAILABLE
        - FORWARD
        - RESERVED
      example: AVAILABLE
    asofDate:
      description: Reference date of the balance
      type: string
      format: date
      example: '2018-12-20'
    amount:
      $ref: '#/definitions/MonetaryAmount'
```

### C

**Country**: Country identifier **MUST** use ‘ISO 3166-1 alpha-2’

``` notoggle
country:
 description: Unique country identifier. Adheres to ISO3166 - Alpha 2
 type: string
 pattern: '^([A-Z]{2,2})$'
 example: US
```

**Currency**: Currency identifiers **MUST** adhere to ISO4217

``` notoggle
currency:
 description: Unique currency identifier. Adheres to ISO4217
 type: string
 pattern: '^([A-Z]{3,3})$'
 example: USD
```

### D

**Dates**: all dates **MUST** use **ISO 8601**.

``` notoggle
effectiveDate:
  type: string
  format: date
  description: 'The effective date for the exchange rate.The acceptable value for the date is as defined by ISO-8601. If the field is empty or the  date is missing, it implies immediate effect as per the current business date of the Finastra subscribers.'
  example: '2018-12-24'
  minLength: 10
  maxLength: 10
```

### E

**Email Addresses**: Error structures **SHOULD** be defined as below.
The type field is included to imply that a party (enterprise or
personal) can have multiple email addresses. The email address length is
256 long, as per ISO20022, and a pattern and/or format mask is not
specified. Enum values may be added and removed as required.

``` notoggle
EmailAddresses:
  title: Email Addresses
  type: array
  description: Email addresses.
  items:
    $ref: '#/definitions/EmailAddress'
EmailAddress:
  title: Email Address
  type: object
  description: Email contact addresses.
  required:
    - type
    - address
  properties:
    type:
      description: Email type. The list below is indicative rather than exhaustive.
      type: string
      example: HOME
      enum:
        - HOME
        - WORK
        - OTHER
    address:
      description: Email address.
      type: string
      maxLength: 256
      example: OfficeAdmin@OfficeAddress.com
```

**Error**: Error structures **MUST** be defined as below. This follows
RFC7807 with only the title and status fields being optional.

``` notoggle
Error:
  title: Error
  type: object
  description: Used as a standard error message structure that complies with RFC7807
  required:
    - title
    - status
  properties:
    type:
      type: string
      description: 'A URI reference [RFC3986] that identifies the problem type'
      example: 'https://api.finastra.com/validation-error'
    title:
      type: string
      description: A short human-readable summary of the problem type
      example: The request is invalid
    status:
      type: integer
      format: int32
      description: The HTTP status code generated by the origin server for this occurrence of the problem
      example: 400
    detail:
      type: string
      description: A human-readable explanation specific to this occurrence of the problem
      example: The account does not exist
    causes:
      type: array
      description: An optional array of additional causes associated with the error
      items:
        $ref: '#/definitions/Causes'
Causes:
  title: Causes
  type: object
  description: Used to define additional causes associated with the error when the associated error message does not provide sufficient clarity on remediation of the error
  properties:
    title:
      type: string
      description: A short human-readable summary of the problem associated with this cause of the error
      example: The account '1234567890' is dormant
    field:
      type: string
      description: The field associated with this cause of the error
      example: account
    fieldValue:
      type: string
      description: The value of the field associated with this cause of the error
      example: 0543123467083
```

### F

**Frequency Codes**: Frequency codes **SHOULD** use the following
structure, however, note that:

-   The frequencies defined above are named as adjectives e.g. a yearly
    event - this should be considered carefully when adding further
    frequency values
-   The frequency code field is anticipated to be used with other fields
    to determine a schedule of events e.g. the frequency code can be
    used with a preferred day and a non-business day adjustment flag to
    request that weekly payments are always made on either a Friday or
    the business day before the Friday
-   Where a specific implementation of the API does not support one or
    more of the frequency enum values then the enum values can be
    removed as required
-   Where a specific implementation of the API supports one or more
    frequency enum values that are not listed then additional enum
    values can be added as required as long as they adhere to the same
    naming convention

``` notoggle
FrequencyCode:
  title: Frequency Code
  description: |
    The frequency code specifies when regular business events occur.
    The following frequency codes supported:
    - AD-HOC - the event is not regular and is manually scheduled
    - DAILY - the event occurs once every business day
    - WEEKLY - the event occurs once every week
    - FORTNIGHTLY - the event occurs once every two weeks
    - HALF-MONTHLY - the event occurs twice every month
    - MONTHLY - the event occurs once every month
    - TWO-MONTHLY - the event occurs once every two months
    - QUARTERLY - the event occurs once every quarter
    - HALF-YEARLY - the event occurs twice every year
    - YEARLY - the event occurs once every year
  type: string
  enum:
    - AD-HOC
    - DAILY
    - WEEKLY
    - FORTNIGHTLY
    - HALF-MONTHLY
    - MONTHLY
    - TWO-MONTHLY
    - QUARTERLY
    - HALF-YEARLY
    - YEARLY
```

### P

**Personal Details**: To represent a personal party, this model
**SHOULD** be used as far as possible. Additional fields may be added as
required.

``` notoggle
PersonalDetails:
  type: object
  description: Personal customer details.
  required:
    - firstName
    - lastName
  properties:
    title:
      type: string
      description: The title used by the person. The list below is indicative rather than exhaustive.
      enum:
        - Mrs
        - Miss
        - Ms
        - Mr
        - Master
        - Doctor
        - Professor
      example: Doctor
    firstName:
      type: string
      description: Person's first name. Alternative names are given name or personal name.
      maxLength: 35
      example: John
    middleName:
      type: string
      description: Person's middle name if available.
      maxLength: 35
      example: James
    lastName:
      type: string
      description: Person's last name. Alternative names are surname or family name.
      maxLength: 35
      example: Walker
    birthDate:
      type: string
      description: Person's birth date.
      format: date
      example: '1975-05-01'
```

**Phone Numbers**: Finastra Open APIs should adopt the following
structure and naming convention when defining phone numbers. The type is
shown to imply that a party (enterprise or personal) can have multiple
phone numbers. The phone number length is 20 long and a format mask is
not specified. Enum values may be added and removed as required.

``` notoggle
PhoneNumbers:
  title: Phone Mumbers
  type: array
  description: Phone contact numbers.
  items:
    $ref: '#/definitions/PhoneNumber'
PhoneNumber:
  title: Phone Number 
  type: object
  description: Phone contact numbers.
  required:
    - type
    - number
  properties:
    type:
      description: Phone number type. The list below is indicative rather than exhaustive.
      type: string
      example: MOBILE
      enum:
        - MOBILE
        - HOME
        - WORK
        - FAX
        - TELEX
        - TELEX-ANSWERBACK
        - OTHER
    number:
      description: Phone number.
      type: string
      maxLength: 20
      example: 0044 01753 573244
```

<!-- 
# Open API Patterns {.unnumbered #rest-patterns}


Finastra APIs should ensure that they follow relevant Finastra Open API patterns.

The following is a set of useful references related to patterns:

1. References to the following Finastra Open API patterns:

* [Concurrency](https://almtools/confluence/display/ARPP/Open+APIs+-+Distributed+Concurrency)
* [Idempotency](https://almtools/confluence/display/ARPP/Open+APIs+-+Idempotency)
* [Caching](https://almtools/confluence/display/ARPP/Open+APIs+-+Caching)
* [Rate Throttling](https://almtools/confluence/display/ARPP/Open+APIs+-+Throttling)

2. References to the following external Open API patterns and anti-patterns:

* [Resource Modelling](https://www.thoughtworks.com/insights/blog/rest-api-design-resource-modeling)
* [Anti-Patterns](https://nordicapis.com/a-pragmatic-take-on-rest-anti-patterns/)


The remainder of this sections provides details on the following patterns:

* [Concurrency](https://almtools/confluence/display/ARPP/Open+APIs+-+Distributed+Concurrency)
* [Idempotency](https://almtools/confluence/display/ARPP/Open+APIs+-+Idempotency)
-->

# Useful Resources

<!-- # Resources {-} -->

#### References

This guide has been produced by taking advantage of various open source
documents.

-   [REST, by Roy
    Fielding](https://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm)
-   [Google Cloud API](https://cloud.google.com/apis/design/)
-   [National Bank of Belgium REST API Design
    Guide](https://github.com/NationalBankBelgium/REST-API-Design-Guide/wiki)
-   [Apigee REST
    programming](https://academy.apigee.com/index.php/courses/elearning/fundamentals-restful-api-design_)
-   [Nordic API practitioner](https://nordicapis.com/)

#### Tools

The [OpenAPI github
repo](https://github.com/OAI/OpenAPI-Specification/blob/master/IMPLEMENTATIONS.md)
provides a list of projects with implementation like editor, validator.

The OpenAPI Specification was originally known as Swagger, before being
contributed to the Open API Initiative.
[Swagger.io](https://swagger.io/) continues to support the community
with both free and commercial tooling. The [Swagger
Editor](https://editor.swagger.io//) is a web-based editor which can
assist with the initial authoring of APIs.

[Swashbuckle](https://github.com/domaindrivendev/Swashbuckle) is a
collection of OAS tools for .NET web APIs. Your service code can be
annotated to facilitate the generation of an OAS definition, and also to
generate the documentation on demand. This method of documentation can
be advantageous when it becomes difficult to keep the OAS definition in
sync with the code.

#### Training

Pluralsight Training

-   [Web API
    design](https://app.pluralsight.com/library/courses/web-api-design/)
-   [Azure API Management
    Concept](https://app.pluralsight.com/library/courses/microsoft-azure-api-management-essentials)
-   [Oauth2/OpenIdConnect/JWT](https://app.pluralsight.com/library/courses/oauth2-json-web-tokens-openid-connect-introduction/table-of-contents)
-   [RESTFul](http://academy.apigee.com/index.php/courses/elearning/fundamentals-restful-api-design)

#### Websites

-   [OpenAPI
    Specification](https://github.com/OAI/OpenAPI-Specification)
-   [Example](https://github.com/NationalBankBelgium/REST-API-Design-Guide/wiki)
-   [Programmable Web API
    university](https://www.programmableweb.com/api-university)

#### Security

-   [HTTP headers
    explains](https://developer.mozilla.org/en-US/docs/Web/HTTP)
-   [REST Security Cheat
    Sheet](https://www.owasp.org/index.php/REST_Security_Cheat_Sheet)

# Document History

<!-- ## Document History -->

<table>
<colgroup>
<col style="width: 10%" />
<col style="width: 17%" />
<col style="width: 71%" />
</colgroup>
<thead>
<tr class="header">
<th style="text-align: left;">Version</th>
<th style="text-align: left;">Date</th>
<th style="text-align: left;">Description</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td style="text-align: left;">2.2</td>
<td style="text-align: left;">January 2020</td>
<td style="text-align: left;"><p>Revised pagination section to remove page concept.</p>
<p>Included the following new sections:</p>
<ul>
<li><a href="#concurrency">Concurrency</a></li>
<li><a href="#idempotency">Idempotency</a></li>
</ul></td>
</tr>
<tr class="even">
<td style="text-align: left;">2.1</td>
<td style="text-align: left;">August 2019</td>
<td style="text-align: left;">Removed the following sections: API Checklist Open API Patterns</td>
</tr>
<tr class="odd">
<td style="text-align: left;">2.0</td>
<td style="text-align: left;">May 2019</td>
<td style="text-align: left;"><p>Renamed as Finastra Open API Design Guide to reflect strategy.</p>
<p>Resequenced and reworded sections for clarification</p>
<p>Included <strong>Appendix</strong> with sections containing:</p>
<ul>
<li><a href="#standards-summary">Standards Summary</a></li>
<li><a href="#rest-dictionary">Dictionary</a></li>
<li><a href="#rest-patterns">Patterns</a></li>
<li>Document History</li>
</ul>
<p>Included the following new sections:</p>
<ul>
<li><a href="#glossary">Glossary</a></li>
<li><a href="#openapi-principles">Open API Principles</a></li>
<li><a href="#rest-customisations">Customisation</a></li>
</ul>
<p>Modified as follows:</p>
<ul>
<li>Removed references to verbs e.g. search in URLs.</li>
<li>Pagination section - added field blocks.</li>
</ul></td>
</tr>
<tr class="even">
<td style="text-align: left;">1.0</td>
<td style="text-align: left;">June 2018</td>
<td style="text-align: left;">First published.</td>
</tr>
</tbody>
</table>

<!-- # CRUD sample {-} -->
<!-- ```{r child="md/rest-crud-interfaces.md"} -->
<!-- ``` -->
<!--chapter:end:part-appendix.Rmd-->
