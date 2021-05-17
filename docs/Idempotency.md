# Idempotency

In the context of REST APIs idempotency refers to the capability of a
system to safely retry identical requests without performing the same
operation twice. That way making multiple identical requests has the
same effect as making a single request. This capability is necessary
when HTTP is used as a transport mechanism because network failures will
occur and responses will be lost, albeit infrequently. Hence, the
ability to retry requests safely adds resiliency to the system. For
example, if the response to a payment request is not received by a
client then if the server has implemented idempotency the client can
safely retry the request without making the payment twice.

The following indicates which HTTP methods are idempotent when RESTful
API principles have been followed:

-   GET, PUT, HEAD, TRACE and OPTIONS methods are idempotent since multiple,
    identical requests will behave the same
-   DELETE can pragmatically be seen as idempotent since the server
    state is unchanged following identical requests
-   POST is not idempotent since repeated requests will create a new
    resource and return a unique resource identifier for each request
    

Finastra APIs should be designed to ensure that POST requests are
idempotent - this is done by the client specifying the following HTTP
header when making POST requests:

-   `Idempotency-Key` - this HTTP header field is a string that contains
    a unique idempotency key, ideally a v4 UUID.

When performing a request, a client generates a unique ID ``idempotency-key`` to identify just that operation and sends it up to the server along with the normal payload.

The server receives the ID and correlates it with the state of the request on its end.

If the client notices a failure, it retries the request with the same ID, and from there it’s up to the server to figure out what to do with it.

Where idempotency is implemented, the backend server must save the
status code and body of the response against the idempotency key,
regardless of whether it succeeded or failed. Subsequent requests with
the same key must then return this stored response. Note that the
response is only stored if the backend started processing the request as
part of a transaction.

The backend server must also consider the following implementation
details:

-   The ```Idempotency-Key``` provided in the header must be at most 40 characters in size. 
    If a larger Idempotency-Key length is provided, the server must reject the request 
    with a status code is 400 (Bad Request).
-   The Identity provider must not change the request body while using the same ```Idempotency-Key```. 
    If the Identity provider changes the request body, the server must not modify the end resource. 
    The server may treat this as a fraudulent action.
-   The value of the `Idempotency-Key` can be the same value as an
    existing payload field as long as the payload field is guaranteed to
    be unique.
-   The server must treat a request as idempotent if it had received the first request with the same ```Idempotency-Key``` 
    from the same Identity provider in the preceding 24 hours.
-   Publish the longevity of idempotency keys - keys are typically
    purged after 24 hours.
-   To ensure that the client is not generating duplicated idempotency
    keys, it may be beneficial to compare incoming request payloads with
    stored request payloads and return an error if they differ.
-   The `Idempotency-Key` header is not the same as the `X-Request-Id`
    header.
-   The server must not create a new resource for a POST request if it is determined to be an idempotent request.
-   The server must respond to the request with the current status of the resource (or a status which is at least
    as current as what's available on existing online channels) and a HTTP status code of 201 (Created).
-   The Identity provider must not use the idempotent behavior to poll the status of resources.
-   The server may use the message signature, along with the Idempotency-Key to ensure that the request body has not changed.

The client must also consider the following implementation details:

-   The client must specify a unique value for `Idempotency-Key`.
-   The client is expected to implement a retry strategy if a **5xx**
    message is received or if a response is not receieved

**Finastra API Standards**

The implementation of idempotency using `Idempotency-Key` is not
mandatory for all APIs since it imposes strict rules on the client and
server, however, full consideration of the implications of NOT
implementing idempotency must be considered, hence:

> Idempotency MUST be considered for all API requests

> Finastra APIs **SHOULD** support idempotency for POST methods

> Finastra APIs supporting idempotency **MUST** define an
> \`Idempotency-Key header on associated POST operations

![idempotency http](images/idempotency_http_flow.png)

**Implementation**

To implement and test all the OpenAPI patterns the Postman tool is selected and screenshots from the Postman illustrate the client and server provisions for a pattern.

So, the implementation of idempotency keys on mutating endpoints (i.e. anything under POST in this case) by allowing clients to pass a unique value in with the special header,
which allows a client to guarantee the safety of distributed operations:

``` notoggle
 curl https://api.finastra.com/sb/v1/withdraw \z
   -u sk_test_BQokikJOvBiI2HlWgH4olfQ2: \
   -H "Idempotency-Key: A8Z5MHwQS7jUmZ" \
   -d amount=2000 \
   -d currency=usd \
   -d description="test" \
   -d customer=cus_AGJ6FJMkGQIpHUTX
```

![idempotency http](images/idempotency_postman.png)

In the sample of the network failure case it works as follows:

-   On retrying a connection failure, on the second request the server will see the ID for the first time, and process it normally.
-   On a failure midway through an operation, the server picks up the work and carries it through. The exact behavior is heavily dependent 
    on implementation, but if the previous operation was successfully rolled back by way of an ACID database, it’ll be safe to retry it wholesale. 
    Otherwise, state is recovered and the call is continued.
-   On a response failure (i.e. the operation executed successfully, but the client couldn’t get the result), the server simply replies with a cached result of the successful operation.

If the request fails due to a network connection error, customer can safely retry it with the same idempotency key, and the customer is charged only once.

