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