# Openess / Closeness

Each layer can be either:

* closed: a request moves from layer to layer immediately below it. It cannot skip a layer.
* open: a request can skip a layer and access one several layer below it.

When no business value is added by a layer below it, it can seem cumbersome to go through it. Skipping it is known as the "fast lane reader" and is possible only if layers are open.

## Isolation

This means that a change made in one layer shouldn't impact a component of another layer (if the interface doesn't change) as each layer as no knowledge about the plumbing of other layers.

To enable this, layers must be closed. 

If layers are open, it will generate a tightly coupled appliation with dependencies between all components. Applications done this way become brittle, difficult to maintain and expensive to change.

## Usage of opened layers

As counter-intuitive as it might be, open layers has a lot of drawbacks but they can still be useful to add better isolation.

When we have a set of utility classes that need to be shared only by the layer itself, if we add this set of classes in the current layer, it means they will be available to the layer above it. 

Instead we can create a new layer above the current one and mark it as open. It means the current layer can use it whenever it needs, it can also decide to skip it and directly access the layer above it. It also means the layer above it can no longer access it because it can't bypasse the current layer.

Example: 

```text
Presentation 
     ↓
Business & Utility classes (closed)
     ↓
Persistence (closed) 
     ↓
Database (closed)
```

Here the Presentation layer can only access the Business but because the Utility classes are in the same layer, it can also access all utility classes.

```text
Presentation 
     ↓
Business (closed)
     ↓
Utility classes (open)
     ↓
Persistence (closed) 
     ↓
Database (closed)
```

Now, the Presentation layer can only access the Business layer but no longer access the Utility Classes. Still, the Business layer can access the Utility classes or go through the Persistence layer directly if needs be
