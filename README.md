# Apollo Federation @interfaceObject bug

I was having problems with composing my subgraphs using @interfaceObject, and and my schema looks very much like your [example](https://www.apollographql.com/docs/federation/federated-types/interfaces/#example-schemas). So I created a reproduction.

Using Rover 0.22.0

Running `rover supergraph compose --config ./supergraph.yml > supergraph.graphql`

```
⌛ resolving SDL for subgraphs defined in ./supergraph.yml
🎶 composing supergraph with Federation v2.3.2
HINT: [INCONSISTENT_INTERFACE_VALUE_TYPE_FIELD]: Field "Media.title" of interface type "Media" is defined in some but not all subgraphs that define "Media": "Media.title" is defined in subgraph "graph-a" but not in subgraph "graph-b".
HINT: [INCONSISTENT_INTERFACE_VALUE_TYPE_FIELD]: Field "Media.reviews" of interface type "Media" is defined in some but not all subgraphs that define "Media": "Media.reviews" is defined in subgraph "graph-b" but not in subgraph "graph-a".
```

I thought the whole point of @interfaceObject was that different subgraphs could contribute different fields to an interface.

It appears as if it does successfully generate a schema, though.


Rover dev has a different problem (besides the fact it is ignoring the federation version in supergraph.yml):

```
> rover dev --supergraph-config ./supergraph.yml
⚠️  Do not run this command in production! ⚠️  It is intended for local development.
🛫 starting a session with the 'graph-b' subgraph
🛫 starting a session with the 'graph-a' subgraph
🎶 composing supergraph with Federation v2.6.1
error[E029]: Encountered 1 build error while trying to build a supergraph.

Caused by:
    INTERFACE_OBJECT_USAGE_ERROR: Type "Media" is declared with @interfaceObject in all the subgraphs in which is is defined (it is defined in subgraph "graph-b" but should be defined as an interface in at least one subgraph)
```

Any ideas?
