# Service-Based architecture

## Overview

It's one of the most pragamatic architecture styles, mostly due to its flexibility.
It's a distributed architecture but is simpler than other distributed architectures like micro-service or event-driven.

It's usually composed of an interface, several domain services, and a monolithic database.

## Components

### Services

Services are deployed separately in this architecture and called "domain services".
They are truly independent, coarse-grained, portion of an application. 
They should never call each other. There are usually less than 10 services to make up a full application.

Services being coarse-grained means and don't talk to each other means we have a good data integrity and consistency but it makes testing harder as the entire service needs to be tested before deploying. 
There are more risk to break something as a deployment will impact a substantial part of the application.

### Interface

Interface is also deployed separately and can communicate with services by calling them directly using remote procedure calls (REST/gRPC/....) but, most of the time, a gateway or reverse-proxy is added between them to unify the API layer.

### API Gateway

There are cases where the API gateway might need to orchestrate calls to several domain services. 
In these cases, the API Gateway needs to implement a Saga or BASE transaction pattern whereas ACID transactions will be used inside each domain services.

### Database

On the other side the database is often deployed as a single monolith and shared between the different services. 
This allows the application to keep an overall data consistency by leveragin SQL queries, joins, ... in the services. 
As an application contains ~10 services, the database connection pool is usually not an issue. 
A logic needs to be put in place, to easily determine which service is owner of which data and make sure other services don't update it. 
It will also make updating the database schema harder as it can impact other services.

One way to handle the entity objects is to use a library shared by all the services.
These libraries, sometimes, also contains SQL code. 
It's a working solution but it's not effective as it will force to deploy all services each time an entity as to be updated, even if the service doesn't really use the table.

One solution to mitigate the impact of changing the database schema is to partition the database logically and split the shared library in smaller shared libraries.

## Conclusion

### Advantages

* Agility: changes are faster than using a Layered architecture
* Testability: better than a Layered application as the scope is smaller, it also runs faster
* Deployment: more frequent deployment as it's less risky than a Layered application
* Fault tolerance: services are truly independent so even if they are coarse-grained, if one service is down, the rest of the application can still works
* Scalability: it's not the most efficient architecture but still better than a Layered application
* Simplicity: the simplest and less expensive distributed architecture to implement
* Reliability: probably the most reliable distributed architecture as services are isolated. 
  It means less distributed transactions, network calls, bandwith used.
* Good fit for domain-driven design as domains services are coarse-grained and domain scoped.

### Drawbacks

* Granularity: it's hard to find the good granularity to make services truly independent without being too big.
* Testability: it's harder to test the interface or the API-Gateway as it depends of serveral different domain services.
