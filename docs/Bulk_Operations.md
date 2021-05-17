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