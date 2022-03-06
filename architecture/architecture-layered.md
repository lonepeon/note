# Layered Architecture

> Also known as n-tiered architecture

### Overview

It's a natural fit for most developers due to [Conway's Law](https://en.wikipedia.org/wiki/Conway's_law). In most organization there are UI developers, Backend developers, database experts, ... and thus we slice the applications in horizontal layers, where each layer has a specific role: Presentation / Business / Persistence. Usually the number of layers is 3 or 4.

The architecture is partitioned by technical roles rather than groupd by domain, as a result business domain is scattered across each layer.

### Deployment units

Possible deployment topologies:

* Presentation / Business / Persistence in one unit. Database in an other
* Presentation in one unit, Business and Persistence in another. Database in a third
* Presentation / Business / Persistance and Database in one unit using in memory / embedded databases

### Pitfalls

#### Skinhole anti-pattern

This anti-pattern occurs when requests move from layer to layer without adding business logic. All layered applications will have some this but it needs to be a low percentage of the requests. The 80-20 rule is a good practice to follow. If too much of ths anti-pattern is at play, it means it's time to change the architecture style (or open layers but this would lead to a brittler application). 

#### Growth

The main strengths of this architecture at lower scale (cost and simplicity) decrease very quickly when the application starts to grow.

### Conclusion

#### Advantages

* This architecture is a good when:
  * we don't know what kind of architecture will be used yet. In this case, keep reuse at minimum and flatten all object hierarchies to maintain a good level of modularity. This will help move to another architecture style later
  * we need a small & simple application / website.
  * we have a tight budget or time constraint because of its simplicity and familiarity amongst developers.
* Cost: we only need one deploment unit, we don't need complex instrumentationm big, network bandwith etc... but be aware it won't last if the application grow as it will scale vertically more than horizontally.
* Simplicity: it's a pattern already well know but be aware it won't last if the appliation grow as everything will be entangle in one bundle and harder to maintain / change.
* Testability: It's easy to test and mock as components are all local but be aware it won't last if the application grow as the test suite will be slower and slower. At this point, it's not rare to bypass the test suite when it's only a 3 line changes which leads to even more disasters.

#### Drawbacks

* Deployability: as the application embeds everything, it has bigger risk to break something on deploy and thus the tendency is to deploy less frequently.
* Reliability: even if there are no network communication, latencies, etc... because all calls are locals, they are all bundled together. It means that if one component fail, everything fail.
* Elasticity / Scalability: because of it's lack of modularity, it can only scale as much as its less scalable component.
* Fault tolerance: because everything is bundled together, the average [MTTR](https://en.wikipedia.org/wiki/Mean_time_to_recovery) is usual bad: deployment and boot time are slow.
