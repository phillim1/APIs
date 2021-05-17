# Localisation and Internationalization

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
