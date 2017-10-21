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
  * isolated
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
* **V(E)** a set of directed edges where each edges is associated with ab ordered pair of vertices.

If an edge e is associated with a pair of vertices (v,w) then e is said to be the directed edge from v to w.


### Simple graph

A simple graph is a graph that does not contain any loops or parallel edges.


### Complete graph

A complete graph on n vertices, denoted Kn, is a simple graph with n vertices and exactly one edge connecting each pair of distinct vertices where n is positive integer ( n > 0 ).


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



## References

<!-- Last, First M. Book. City: Publisher, Year Published. Print. -->

[Discrete Mathematics with Application. Fourth Edition. Susanna S. Epp](https://www.amazon.co.uk/Discrete-Mathematics-Applications-International-Susanna/dp/0495826162/ref=sr_1_1?ie=UTF8&qid=1508619365&sr=8-1&keywords=9780495826163)
