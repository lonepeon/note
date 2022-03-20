# Distributed Tracing

## Set proper trace attributes

When declaring the tracing provider, don't forget to set:

1. the `service.version` attribute based on the current version or Git SHA.
2. the `service.name` attribute which is the (micro-)service name
3. the `service.namespace` which is the project name the service is part of
4. the `service.id` which is the current service instance (EC2, Kubernetes Pod ID, ...)

## Set proper span kind

1. To set the `span` as a `client` when we receive a call from the outside
2. To keep the `span` as `internal` when we are inside the application
3. To set the `span` as `server` before calling an external service.

## Span and log best practices

Span names and log messages should be static. 
To make it clear: interpolation shouldn't be used. 
It makes it harder to later search / aggregate data if they are dynamic. 
For dynamic data, attach attributes to the span or logs.

Attribute keys should be as static as possible and rely on semantic conventions.

```jsonc
// 🛑 it makes it harder to filter and don't follow semantic conventions
{
    "customer": "42", // or "admin", ...
}

// ✅ is a better practice
{
    "enduser.id": "42",
    "enduser.role": "customer" // or "admin", ...
}
```

## To span or to log events

A `span` should be created if:

1. the code enters an important zone of the application (handler, business logic)
2. it's important to time the speed of the code (external API call, database call, ...)

A `log` should be preferred if:

1. it relates to something happening (the code enters this condition or this other condition)
2. it adds context on the current span like the error message received from an API Call, the executed SQL query, ...)

> ℹ️ **info**
> 
> The suggestion is to default on logging event first. 
> It's always possible to create a span later. 
> Adding to much span can make it harder to follow what's happening in the service call.
