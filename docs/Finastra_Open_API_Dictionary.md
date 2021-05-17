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
