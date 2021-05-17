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