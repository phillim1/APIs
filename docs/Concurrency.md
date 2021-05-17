# Concurrency

## Statement
A concurrency conflict occurs when a user tries to update a resource which has been modified 
after the last time the user last retrieved that resource. The consequence of this is that 
the user can unknowingly overwrite another user's update to the resource. This is a type of
race condition and it leads to problematic behaviors, which are difficult to detect and to debug. 
There is no way to deal with this problem without rejecting one of the user's updates. 
However, lost updates and race conditions must be avoided.

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

## Scenarios
There are two potential scenarios for concurrency control:

- Pessimistic concurrency control: The system locks the data record as long as a client works with it and prevents any modification by another client. Pessimistic concurrency control can lead to performance issues
- Optimistic concurrency control: Together with the data itself a versioning token is given to the client. When trying to modify the record the systems checks if this token is still valid, which means no change took place in the meantime. The change is only applied if no conflict is detected


Distributed systems are typically stateless, thus a pessimistic solution does not  fit. One reason for this is that most distributed systems use REST and / or at least HTTP for communication – which itself is already stateless.

Therefore the optimistic concurrency control is the preferred approach.

## Applicability
This pattern is applicable for any stateless distributed systems using REST style over HTTP protocol. A system can be presented by client Web Application, REST API server and a backend database (optional).

The following considerations detail the applicability of the pattern:

1. REST over HTTP synchronized communication is considered as a distribution integration pattern between client and backend service

2. Optimistic concurrency control is considered to update a client with the version as described below

3. Similar to the Cache pattern - Conditional HTTP ETag and If-Match are used for the client updates  

## Solution and Structure

### Optimistic locking
From a concurrency control strategy implementation optimistic locking is preferred. Optimistic locking functions as follows: try to do an operation, then fail if the resource has changed since last seen. Optimistic locking is different to pessimistic locking in which a resource is locked from alteration before any changes are done and then released after changes are done. As pessimistic locking schemes require state to be kept at the server it does not align well in a RESTful APIs where it is idiomatic to keep the server stateless.

The general approach for optimistic locking is to send a value, along with the payload, that identifies which version of the resource should be modified. If the version to modify is stale, the request should fail. The version identifier could be an explicit version number or hash of the unmodified resource. Last modified timestamps could also be used, but with caution as clocks might be skewed in a distributed environment.

As a mechanism of concurrency control optimistic locking is a simple yet powerful approach. Implementing it for the API-creator is relatively straightforward. This approach does however impose the risk of end-user’s suffering due to the race condition. A strict concurrency control strategy requires that the entire end-to-end flow adhere to it. The integrator would be forced to implement graceful resolving of conflicts due to the concurrency control — likely a non-trivial task, definitely so if a GUI is involved.

### Conditional HTTP Headers
“ETags can also be used for optimistic concurrency control, as a way to help prevent simultaneous updates of a resource from overwriting each other.” (Wikipedia)

The following RFC defines the Conditional HTTP Headers:

- [HTTP](https://tools.ietf.org/html/rfc7232/) - Hypertext Transfer Protocol (HTTP/1.1): Conditional Requests

ETag may contain a hash value of the resource’s content, a hash of the last modification timestamp or even just a revision number.

The service consumer must cache the received versioning information and when calling a PUT operation add it to the request header field If-Match.

Identifying the version in a HTTP request one can either put the version identification as part of the request model in the payload or utilize the If-Match request-header from the HTTP/1.1 specification. Note that the HTTP/1.1 specification requires 412 (Precondition failed) to be returned upon conflict.

## Participants

|Participant | Description and responsibility |
|-------:|----------: |
|API caller (Client 1)| Web or mobile application, for example, user 1|
| API caller (Client 2)| The same application, user 2 |
|Source of Records (Optional) | Relational database, Oracle, for example|
| Server | Backend API Provider capable to generate the Cache HTTP headers listed below |

## Collaborations

![ Conditional HTTP Headers for concurrent workflow](images/concurrency_conditional_http_headers.png)

The flow steps are as follows:

1. Client 1 issues GET /api.finastra.com/pl/v1/doc request.
2. Client 2 issues GET /api.finastra.com/pl/v1/doc request.
3. A resource sends the versioning information in the response ETag header as well as Last-Modified header to Client 2
4. A resource sends the versioning information in the response ETag header as well as Last-Modified header to Client 1
5. Client 1 uploading the modified document with PUT /api.finastra.com/pl/v1/doc request
6. A resource sends the response to Client 1.
7. Client 2 uploading the modified document with PUT /api.finastra.com/pl/v1/doc request
8. A resource sends the response to Client 2.

It looks like a straightforward flow with two independent users (clients) accessing a resource to GET the response, update data locally, and inform the server about local updates with the PUT request.

Unfortunately, things get a little bit more complicated as soon as concurrency is taken into account. While a client is locally modifying its new copy of the resource, a second client can fetch the same resource and do the same on its copy. What happens next is very unfortunate: when they commit back to the server, the modifications from the first client are discarded by the next client push, as this second client is unaware of the first client's changes to the resource. The decision on who wins, is not communicated to the other party. Which client's changes are to be kept, will vary with the speed they commit; this depends on the performance of the clients, of the server. This is a race condition and leads to the very problematic behavior. 

The next diagram details the problem.

![race condition problem](images/concurrency_race_condition_problem.png)

## Implementation
412 Error Activity Diagram below details the HTTP 412 (Precondition Failed) error generation.

The If-Match header makes the request conditional exactly as for the Cache pattern. Backend server (API provider) must check if the condition “resource was not modified” is fulfilled. If the condition is satisfied, that means the update is possible, the HTTP status code 200 (OK) with the updated resource as payload is sent.

If, however, the update cannot be applied because the resource was modified in the meantime the service will return the status code 412 (Precondition Failed). Receiving that status the client has to decide how to handle the conflict – by reloading the resource and discarding or merging the changes.

![concurrency_conditional_412_error](images/concurrency_conditional_412_error.png)


Therefore, the conditional requests allow implementing the optimistic locking algorithm as shown above. Then the sequence diagram with the race condition can be corrected to avoid lost updates as follows:

![sequence diagram](images/concurrency_sequence_diagram.png)

This may be implemented using either If-Match or If-Unmodified-Since-Match headers. Activity diagram above shows If-Match flow only.

HTTP ETag  header prevents the simultaneous updates of a resource. If the ETag  doesn't match the original file, or if the file has been modified since it has been obtained, the change is simply rejected with a 412 Precondition Failed error. It is then up to the client to deal with the error: either by notifying the user to start again (this time on the newest version), or by showing the user a diff of both versions, helping them decide which changes they wish to keep.

When optional database (source of records) component is added into the picture If-Match Activity Diagram above should be enhanced to as follows:

![ Conditional If-Match HTTP Header workflow for concurrent distributed transactions.](images/concurrency_conditional_http.png)


Now the 412 Error Activity Diagram’ flow (Fig.3) is updated with the additional interactions with the database. Even after comparing the received version with the current version – that means after reading the stored record from the database and before updating it – there might be another transaction that changes the record in the very same moment. Therefore it is still possible that an OptimisticLockException occurs. In this case the HTTP status code 409 (Conflict) is returned.

If the update is executed correctly then the status code 200 (OK) is sent to the client.

Implementation Section shows that the end-to-end holistic solution for the distributed transactions with optimistic locking mechanism is indeed non-trivial task.

References:

-   [MDN web docs:
    ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)
-   [RFC 7232, section 2.3:
    ETag](http://tools.ietf.org/html/7232#section-2.3)