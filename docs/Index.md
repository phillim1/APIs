Finastra Open API Design Guide
================
**Fusion**Fabric.cloud Documentation Team
13 April 2021

-   [](#frontpage)
-   [(PART\*) Design Guidelines](#part-design-guidelines)
-   [Introduction to REST](./Introduction_to_REST.md)
    -   [REST](./Introduction_to_REST.md#rest)
    -   [REST Resources](./Introduction_to_REST.md#rest-resources)
    -   [REST and HTTP Transport](./Introduction_to_REST.md#rest-and-http-transport)
    -   [REST and HTTP Methods](./Introduction_to_REST.md#rest-and-http-methods)
    -   [REST and HTTP Headers](./Introduction_to_REST.md#rest-and-http-headers)
-   [Introduction to Open APIs](./Introduction_to_Open_APIs.md)
    -   [Open APIs and OpenAPI](./Introduction_to_Open_APIs.md#open-apis-and-openapi)
    -   [Finastra Open API Principles](./Introduction_to_Open_APIs.md#finastra-open-api-principles)
    -   [API Maturity Levels](./Introduction_to_Open_APIs.md#api-maturity-levels)
-   [Payloads and Formats](./Payloads_and_Formats.md)
    -   [JSON Payloads](./Payloads_and_Formats.md#json-payloads)
    -   [Other Payloads](./Payloads_and_Formats.md#other-payloads)
    -   [XML and Other Standard
        Formats](./Payloads_and_Formats.md#xml-and-other-standard-formats)
-   [Responses and Errors](./Responses_and_Errors.md)
    -   [Finastra Error Message
        Structure](./Responses_and_Errors.md#finastra-error-message-structure)
-   [Naming Conventions](./Naming_Conventions.md)
    -   [Resources](./Naming_Conventions.md#resources)
    -   [Resource Payload](./Naming_Conventions.md#resource-payload)
    -   [Abbreviations](./Naming_Conventions.md#abbreviations)
    -   [Enumerations](./Naming_Conventions.md#enumerations)
-   [Versioning](./Versioning.md)
    -   [Backward compatibility](./Versioning.md#backward-compatibility)
    -   [REST versioning](./Versioning.md#rest-versioning)
-   [Collections and Pagination](./Collections_and_Pagination.md)
    -   [Sorting](./Collections_and_Pagination.md#sorting)
    -   [Searching and Filtering](./Collections_and_Pagination.md#searching-and-filtering)
    -   [Pagination Request](./Collections_and_Pagination.md#pagination-request)
    -   [Pagination Response](./Collections_and_Pagination.md#pagination-response)
    -   [HATEOAS and HAL](./Collections_and_Pagination.md#hateoas-and-hal)
    -   [Dynamic Response Payloads](./Collections_and_Pagination.md#dynamic-response-payloads)
-   [Bulk Operations](./Bulk_Operations.md)
-   [Server Events and Long Running
    Tasks](./Server_Events_and_Long_Running_Tasks.md)
    -   [Long-Polling](./Server_Events_and_Long_Running_Tasks.md#long-polling)
    -   [Webhooks](./Server_Events_and_Long_Running_Tasks.md#webhooks)
-   [Customisation](./Customisation.md)
    -   [Custom Fields](./Customisation.md#custom-fields)
-   [Localisation and Internationalization](./internationalization.md)
-   [Concurrency](./Concurrency.md)
-   [Idempotency](./Idempotency.md)
-   [Glossary](./Glossary.md)
-   [Finastra Standards Summary](./Finastra_Standards_Summary.md)
    -   [Conventions](./Finastra_Standards_Summary.md#conventions)
    -   [Principles](./Finastra_Standards_Summary.md#principles)
    -   [General](./Finastra_Standards_Summary.md#general)
    -   [Names](./Finastra_Standards_Summary.md#names)
    -   [HTTP Methods](./Finastra_Standards_Summary.md#http-methods)
    -   [HTTP Headers](./Finastra_Standards_Summary.md#http-headers)
    -   [HTTP Custom Headers](./Finastra_Standards_Summary.md#http-custom-headers)
    -   [Swagger info - API
        Information](./Finastra_Standards_Summary.md#swagger-info---api-information)
    -   [Swagger host - Serving the
        API](./Finastra_Standards_Summary.md#swagger-host---serving-the-api)
    -   [Swagger security - Security](./Finastra_Standards_Summary.md#swagger-security---security)
    -   [Swagger paths - Operations and
        Resources](./Finastra_Standards_Summary.md#swagger-paths---operations-and-resources)
    -   [Swagger parameters -
        Parameters](./Finastra_Standards_Summary.md#swagger-parameters---parameters)
    -   [Swagger responses](./Finastra_Standards_Summary.md#swagger-responses)
    -   [Swagger tags](./Finastra_Standards_Summary.md#swagger-tags)
    -   [Swagger externalDocs](./Finastra_Standards_Summary.md#swagger-externaldocs)
    -   [Other - Pagination](./Finastra_Standards_Summary.md#other---pagination)
    -   [Other - Performance](./Finastra_Standards_Summary.md#other---performance)
-   [Finastra Open API Dictionary](./Finastra_Open_API_Dictionary.md)
    -   [General Guidelines](./Finastra_Open_API_Dictionary.md#general-guidelines)
    -   [Specific Examples](./Finastra_Open_API_Dictionary.md#specific-examples)
-   [Useful Resources](./Useful_Resources.md)
-   [Document History](./Document_History.md)

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