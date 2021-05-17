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