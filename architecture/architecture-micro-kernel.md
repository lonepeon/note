# Micro Kernel Architecture

> also known as Plug-in Architecture

### Overview

It's a simple monolithic architecture composed of: a core system and plug-in components.

Application logic is scattered between independent plug-in components and the core system. 
This provides extensibility, adaptability, and isolation of application features and custom processing logic but, in the same time, make it harder to reason about.

### Components

#### Core System

The core system can be either:

* the minimal functionality required to run the system. 
  For a text editor, this would be: open, edit, and save a file
* the general processing flow where all complexity has been removed

The core system is usually implemented as a layered arcgitecture.
In some rare cases, it can be split and deployed as separate domains, where each domain would contain its own set of plug-in components.

#### Plug-in

A plug-in component is a standalone, independent component meant to enhance the core system by providing new features or specialize a processing. 
It should be isolated from other plug-in components. 

Plug-ins installation can be:

* Compile-time: Simpler to manage but requires the entire monolith to be redeployed each time a plug-in is added / changed / removed.
* Runtime: Installation is usually done using frameworks making it harder to manage, but the entire monolith doesn't need to be redeployed when a plug-in is added / changed / removed.

### Communication

As a micro-kernel architecture is usually a monolithic application, the communication between the core system and plug-in components is usually done by local procedure calls. 

In some cases this can also be implemented through remote calls (REST API, RPC, Messaging) bringing a better decoupling, scalability and throughput, allow for runtime configuration without relying on special framework. 

This also transforms the system to a distributed system making it more complex to monitor, harder to debug, more prone to network failures, and requiring a much bigger network bandwith. 
A plug-in being unresponsive or a network issue can make the system unresponsive.

These tradeoff needs to be analyzed before chosing one or the other solution.

### Data Access

#### Database

The core system usually doesn't share its database with the plug-in components and rather send all the required data as input of the plug-in component. 
Database changes should only impact the core system.

If a plug-in needs to also store data, it should have its own data store.

#### Contracts

As the plug-in components shouldn't access the core-system database, a contract interface needs to be defined by the core system. 
Usually the contracts are implemented in JSON or XML. 
It can even be object directly sent back and forth. 
The most important criteria here, is to be consistent across all core system domains.

### Conclusion

#### Advantages

* Cost: we only need one deploment unit, we don't need complex instrumentation, big network bandwith etc... but be aware it won't last if the application grow as it will scale vertically more than horizontally.
* Simplicity: having a separation between the core system and the plug-in component make it simple at first but it won't last if the application grows as everything will be entangle in one bundle and harder to maintain / change. 
  It will also be harder to reason about.
* Testability: having a sepration between the core systen and the plug-in components make it easier to test features in isolation. 
  It can also overcome the slow test suite issue of most monolithic applications if plug-in are enabled / deployed at runtime as they can be developed and deployed independently.
* Deployability: if plug-in are deployed at runtime and done correctly, it can be much easier to deploy than most monolithic application as the lifecycle of the core system and each plug-in will be separated. Otherwise it will have the usual monolithic drawback: the application will embed everything and have bigger risk to break something on deploy and thus the tendency is to deploy less frequently.
* Modularity: features can be added / changed / removed without touching the core system, even better, it can be done by using independent components. This will make it relatively easier to extend application and quickly respond to change.
* Performance: these appliactions are usually small and don't grow as big as most layered architecture. If performance starts to take a hit, it can be temporarily fixed by un-plugging some functionalities.

#### Drawbacks

* Elasticity / Scalability: because it's a monolith, it can only scale as much as its less scalable component.
* Fault tolerance: because everything is bundled together, the average [MTTR](https://en.wikipedia.org/wiki/Mean_time_to_recovery) is usual bad: deployment and boot time are slow. It can be improved by using a runtime plug-in system.
