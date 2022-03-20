# REST API

## Verbs

### GET

Can be used in several different ways

* Used to get a resource
  ```text
  GET /order/1337
  ```
* Used to list resources, optionnaly a query string can be used to customize the output
  ```text
  GET /ord#ers
  ```

### POST

Used to create  a new resource

```text
POST /user/42/addresses

{
    "name": "...",
    "line1": "...",
    "line2": "...",
    "line3": "...",
    "state": "...",
    "zipcode": "...",
    "country": "..."
}
```

### PUT

Used to update a resource by sending the full payload, not a parial one. 
It means that, if some of the attributes are not set, they will be replaced with null.

```text
PUT /user/42/address/2

{
    "name": "...",
    "line1": "...",
    "line2": "...",
    "line3": "...",
    "state": "...",
    "zipcode": "...",
    "country": "..."
}
```

### PATCH

Can be used in several different ways

* Used to update part of a resource.
  In this case I would strongly suggest to follow the [json patch RFC](https://tools.ietf.org/html/rfc6902)
  ```text
  PATCH /user/42/address/2
  [
    { "op": "replace", "path": "/zipcode", "value": "..." },
    { "op": "remove", "path": "/line3" }
  ]
  ```
* Used to change a resource state

  ```text
  PATCH /order/1337/dispatched
  ```

### DELETE

Used to remove a resource

```text
DELETE /order/42/address/2
```

## HTTP Status Codes

### Success

* 200: used when an action is not a create and finished successfully
* 201: used when a create action finished successfully
* 202: used when an action will be done but can't give any response yet (batches, background job, ...)
* 204: used when an action finished successfully but no responses has to be sent back (a delete might return nothing)

### Client error

* 400: used when the structure of the body is wrong (missing keys, additional keys, wrong type,...)
* 401: used when the client is not authenticated
* 403: used when the client is not authorized
* 404: used when the resource doesn't exist.
* 422: used when the content of the body is wrong (bad email format, email already takent, ...)

> ⚠️ **warning**
>
> Beware that 404 and 403 should be used consistently otherwise it might help malicious users to determine if a resource exist before trying to gain access to it.
>
> For example, on`/user/<id>/wallet/<id>`if we return 404 when the wallet id doesn't exist and 403 when the wallet exist but is owned by another user, this might help a malicious user.
> 
> We should return either a 403 or 404, none is better than the other as long as the response is consistent.

## Versioning

Several strategies are usually used:

1. URL path or subdomain: this is mostly used as safety net if we really need a big rewrite. 
   Basically we should avoid as much possible to bump this one.
2. Query string: this is the easiest to manipulate as we can copy the URL to someone else and be sure they will use the same version as us but it adds noise into the requests that doesn't belong to the business of the endpoint. 
   Moreover, most of the APIs are uses programmatically so having them in the query string don't bring much value in this case.
3. Header: It  doesn't change the action we want (endpoint / parameters), it just customize the output, the same way we can negotiate the content format using the `Accept` header. 
   We can use it as part of the `Accept` header or use a different header like `Accept-Version` for this.

If I had to choose, I would chose to have the URL path or subdomain as a safety net in case of disaster where we need to rewrite everything but would prefer an API that evolve over time instead of having any of the query or headers type of versioning.

The API evolution rules:

* Don't break the interface:
  * If a field has to be renamed, add a new one and do the extra work for people using the old field to translate it to the new one (e.g if you want a `name` field instead of `firstname` and `lastname`.Use the 3 fields and just merge `fistname` and `lastname` yourselves.)
  * Don't remove fields
  * If a field has to be added, make it optional with a sane default value.
  * If a field has to be removed, don't. :) 
* If an interface really has to be broken, use a migration strategy:
  * create a new endpoint.
  * add the [Deprecation header](https://tools.ietf.org/html/draft-dalal-deprecation-header-03) to the existing endpoint
  * add the [Sunset header](https://tools.ietf.org/html/rfc8594) to specify a date far enough in the future when the endpoint will be removed
  * update the OpenAPI definition to mark the endpoint as deprecated
  * notify all clients to ask them to migrate, if possible, telling them how
  * create some metrics to monitor the usage of this endpoint
  * update the code of the existing endpoint, when possible, to migrate the data in order to call the same business code as the new endpoint

  > As an API consumer, we should monitor the `Deprecation` and `Sunset` headers to notify ourselves if one of the API we use is deprecating an endpoint we use.
  
