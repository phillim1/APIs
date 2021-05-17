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