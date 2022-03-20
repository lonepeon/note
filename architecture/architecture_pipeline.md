# Pipeline Architecture

> also known as pipes and filters or map/reduce

## Overview

It represents the underlying principles behind Unix: create small independant modules that filters or transforms the data and combine these modules to be called one after the other in a sequential way. 
This architecture style encourages code reuse.

* Filters: it represents a task, usually stateless, that does one thing, independently from the others.
* Pipes: it represents the communication channel between each filters. 
  A pipe is unidirectional and point-to-point, accepting input from one source and always redirecting output to another. 
  The payload itself can be of any format but it's better to keep it small for performance reasons.

It's commonly used by:

* [EDI](https://en.wikipedia.org/wiki/Electronic_data_interchange): to tranform one document type to another
* [ETL](https://en.wikipedia.org/wiki/Extract,_transform,_load): to transform the data when moving it from one data source to another
* Orchestrators/Mediators: to pass information from one step in a business process to another

## Type of filters

* Producer: also known as source. 
  It's the starting point of the process
* Transformer: also know as map. 
  It accepts input, transforms it somehow, and return it to the outbound pipe
* Tester: also know as reduce. 
  It accepts input, filters out some based on one or several criteria, and optionally transforms the data. 
  Then it return it to the outbound pipe
* Consumer: It's the termination point of the process. 
  It persists the result somewhere or display it to the user

## Conclusion

### Advantages

* Cost: it only needs one deploment unit, we don't need complex instrumentation, big network bandwith etc... 
  but be aware it won't last if the application grow as it will scale vertically more than horizontally
* Simplicity: it's a monolith and, as such, avoid all the distributed system complexity. 
  Be aware it won't last if the appliation grow as everything will be entangle in one bundle and harder to maintain / change.
* Modularity: Filters are isolated and can be added / updated / removed in total isolation making it very modular.
* Testability: it's easy to test and mock as components are all local.
  It's even easier to test than the layered architecture as filters are isolated from the core system and can be tested in full isolation. 
  Just be aware it won't last if the application grow as the test suite will be slower and slower. 
  At this point, it's not rare to bypass the test suite when it's only a 3 line changes which leads to even more disasters.

### Drawbacks

* Deployability: a bit better than the layered architecture but still a monolith and risky as such
* Reliability: even if there are no network communication, latencies, etc... because all calls are locals, they are all bundled together. 
  It means that if one component fail, everything fail.
* Elasticity / Scalability: because it's a monolith, it can only scale as much as its less scalable component.
* Fault tolerance: because everything is bundled together, the average [MTTR](https://en.wikipedia.org/wiki/Mean_time_to_recovery) is usual bad: deployment and boot time are slow.
