---
layout: post
title: "Graph Theory"
date: 2017-10-21 20:00:00 +0100
categories: maths
---

* [Basic concepts](#basic-concepts)
* [References](#references)

## Basic Concepts

* vertices
  * isolated
  * adjacent
* edges
  * loops
  * parallel
  * adjacent

A graph is a set of _vertices_ and a set of _edges_ that connect vertices. An edge either connects one vertex to another or a vertex to itself.  When an edge connects a vertex to itself it is knowns as a _loop_.  If two edges connect the same set of vertices they are said to be parallel. A vertex is _isolated_ if there are no edges that connect it to another vertex.


### Graph

A graph **G** consists of two finite sets:

* **V(G)** a non empty set of vertices
* **E(G)** a set of edges where each edge is associated with a _set_ consisting of either one or two vertices, which are known as its endpoints.

An edge with just one endpoint is called a loop. Two or more edges with the same endpoints are said to be _parallel_.  An edge connects endpoints; connected endpoints are said to be _adjacent_, a vertex that is an endpoint in a loop is said to be adjacent to itself.  An edge is an incident on its endpoints and two endpoints that are incidents on the same endpoints are said to be _adjacent_.  A vertex that is not an endpoint or is only an endpoint in a loop is said to be _isolated_.


### Directed graph (digraph)

A directed graph or digraph consists of two finite sets:

* **V(G)** a non empty set of vertices
* **V(E)** a set of directed edges where each edges is associated with an ordered pair of vertices.

If an edge e is associated with a pair of vertices (v,w) then e is said to be the directed edge from v to w.


### Simple graph

A simple graph is a graph that does not contain any loops or parallel edges.


### Complete graph

A complete graph on n vertices, denoted Kn, is a simple graph with n vertices and exactly one edge connecting each pair of distinct vertices where n is a positive integer ( n > 0 ).


### Complete bipartite graph

The vertices are separated into two subsets each vertex in one subset is connected by exactly one edge to each vertex in the other subset but not to any other vertices in its own subset.

#### formal definition

A complete bipartite graph on m,n vertices, denoted Km,n, is a simple graph with distinct vertices v1,v2,...,vm and w1,w2,...,wn that satisfy the following:

For all i,k = 1,2,..,m and j,l = 1,2,..,n

1. There is an edge from each vertex vi to each vertex vj.
2. There is no edge from any vertex vi to any other vertex vk.
3. There is no edge from any vertex vj to any other vertex vl.


### Subgraph

A graph H is said to be a subgraph of G if and only if every vertex in H is also in G and every edge that is in H has the same endpoints as it has in G.

Basically H is a subset of the vertices and edges in G. It does not mean that all the edges from G that would be applicable in H are in H just that any edge in H is also in G.


### Degrees

The degree of v, denoted deg(v), equals the number of edges that are incidents on v with an edge that is a loop counted twice.

The total degree of a graph G is the sum of the degrees of its vertices.  It will be twice the number of edges.

#### Handshake theorem

If G is any graph then the sum of the degrees of all the vertices of G will equal twice the number of edges of G.  Specifically if the vertices of G are v1,v2,...,vn where n is a positive integer then:

* total degree of G = deg(v1) + deg(v2) + ... + deg(vn)
* the total degree  = 2 * (number of edges in G)

Each edge contribute 2 to the total degree of G. An edge can be:

* A loop - counted twice
* Not a loop - is an incident on two vertices.

#### The degree of a graph is always even

The total degree of G equals twice the number of edges which by definition means it must be an even number.

## Graph Walks

A walk from V to W is a finite alternating sequence of adjacent vertices and edges.

The *trivial* walk from v to v consists of just the vertex v.

A *trail* from v to w is a walk from v to k that does not contain any repeat edges.

A *path* from v to w is a *trail* that does not contain any repeat vertices.

A *closed walk* is a walk that starts and ends at the same vertex.

A *circuit* is a closed walk that contains at least one edge and does not contain any repeated edges.  i.e. it is a trail with at least one edge.

A *simple circuit* is a circuit that does not have any other repeated vertex except the first and last.  i.e. it is a path that starts and ends at the same vertex.

## Connectedness

A graph is connected if it is possible to travel from any vertex to any other vertex along a sequence of adjacent edges of the graph.

Two vertices v and w are connected if and only if there is a walk from v to w.

A graph is connected only if given any two vertices v and w there is a walk from v to w.

If vertices v and w are part of a circuit and one edge is removed from the circuit then there is still a trail between the two.

If G is connected and G contains a circuit then an edge of the circuit can be removed without disconnecting G.

A Graph H is a connected component of a graph G *if and only if*:

H is a subgraph of G.
H is connected.
No connected subgraph of G has H as a subgraph and contains vertices or edges that are not in H.

i.e the subgraph is the largest it could possibly be.


## Euler Circuits

Let G be a graph. An Euler circuit for G is a circuit that contains every vertex and edge of G.

* It starts and ends at the same vertex
* uses every vertex at least once
* uses every edge exactly once

If a graph has an Euler circuit then every vertex of the graph has a positive even degree.

Proof:

As a Euler circuit contains every edge of the graph and when you enter a value by one edge you must leave by another edge and every incident on a vertex is traversed exactly once. The edge incidents on a vertex can be arranged in entry exit pairs so the degree of each vertex must be a multiple of 2 which means that the degree of each vertex must be even.

There is an Euler circuit if the following conditions are met:

* The degree of every vertex must be a positive even number.
* The graph must be connected.

## References

<!-- Last, First M. Book. City: Publisher, Year Published. Print. -->

[Discrete Mathematics with Application. Fourth Edition. Susanna S. Epp](https://www.amazon.co.uk/Discrete-Mathematics-Applications-International-Susanna/dp/0495826162/ref=sr_1_1?ie=UTF8&qid=1508619365&sr=8-1&keywords=9780495826163)
