# Server Events and Long Running Tasks

This section describes the following REST API approaches to handling
published server events and long running tasks:

1.  [Long Polling](https://www.ably.io/concepts/long-polling)
2.  [Webhooks](https://realtimeapi.io/hub/webhooks/)

Notes: - Webhooks are supported in OAS v3.0 - WebSockets is an
alternative technology that can be used to support events and long
runnning tasks, however, WebSockets is outside the scope of this Guide
as it is not REST API based. For some use cases WebSockets may be the
most appropriate technology and should be explored further as required

## Long-Polling

Long-polling involves a client subscribing to events or requesting the
status of a long running task from a server through a mechanism similar
to polling but with a significant difference being that the client does
not have a ‘polling retry’ timer, rather then client specifies a timeout
period and the server holds a long lived connection.

The server principle is the long running task returns an id and the
response status code
`202 "The request has been accepted for processing, but the processing has not been completed"`.

Then, the client pulls the new created resource and get a `202` code
until the action is completed.

**The workflow**

1.  Request (create task)

``` notoggle
POST /complexReport HTTP/1.1
{...}
```

1.  Response (return task Id)

Return the 202 ‘Accepted’ HTTP response code.

``` notoggle
HTTP/1.1 202 Accepted
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
```

1.  Request (request task status by Id)

``` notoggle
GET /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a HTTP/1.1
Accept: application+json
```

1.  Response (return status or redirect to task return value)

``` notoggle
HTTP/1.1 202 Accepted
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
{
  "status": "..."
}
```

``` notoggle
HTTP/1.1 200 OK
Location: /complexReport/ef07186e-55d4-4db6-b160-f9afaa195d0a
{
  "reportData": [ ... ]
}
```

## Webhooks

Webhooks are HTTP callbacks that receive notification messages for
events.

There are typically two types of Webhooks models:

-   **1-to-1 model**
-   **1-to-many model**

**Letter Box Model - 1-to-1**

Whenever a server event is published, the server makes a REST API call
to the one subscribing client’s API pre-defined endpoint .

**Letter Box Model - 1-to-many**

Whenever a server event is published, the server makes a REST API call
to one or more subscribing clients’ API endpoints.

The server provides API endpoints to allow clients to subscribe to
events so that the client can be informed of the events they register
for.

To provide Webhooks the server needs to implement APIs to allow clients
to:

-   register a Webhook
-   cancel a Webhook
-   optionally - list event types and characteristics that are available
    for clients to subscribe to