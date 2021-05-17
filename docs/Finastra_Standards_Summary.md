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