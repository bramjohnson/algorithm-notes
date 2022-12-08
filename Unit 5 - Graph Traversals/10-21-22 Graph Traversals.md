# Introduction to Graphs
## Directed Graph
A directed graph contains a set of *nodes* $V$ and a set of *edges* $E$.
An edge is an ordered $e = (u,v)$, which represents going from $u$ to $v$

## Undirected Graph
Same definition as directed graphs
An edges is an unordered $e = (u,v)$, which represents going between $u$ and $v$.

## Simple Graph
- No duplicate edges
- No self-loops
	- $e = (u,u)$

# Connectivity
A **path** is a sequence of consecutive edges in $E$, where the *length* of the path is the number of edges.

An *undirected* graph is **connected** if every for every vertex $v$ in $V$ there is a path from $v$ to every other  vertex.
A *directed* graph is **strongly connected** if for every vertex $v$ in $V$ there is a path from $u$ to $v$ AND from $v$ to $u$.

# Cycles
A **cycle** is a path that contains at least one pair of duplicate *vertices*, where all other vertices are *distinct*.

# Degree
In an *undirected* graph, the **degree** of a vertex is the number of edges it holds. In any *undirected* graph with $n$ vertices and $m$ edges, the sum of the vertex degrees is $2m$.

# Example Theorem
"Any connected undirected graph $G$ with $n$ vertices has at least $n - 1$ edges."
We can prove this using *induction*

## Base
Trivial for $n = 1$, a graph with $1$ vertex has $0$ edges.

## Inductive Hypothesis
Assume true for any graph with $m$ vertices
- must have $\geq m-1$ edges

## Contradiction
Suppose we have a *connected* graph $G$ with $m + 1$ vertices and less than $m$ edges.
- By pigeon-hole, there exists a vertex $v$ of degree 0 or 1
- if degree of $v$ is 0, then $G$ is disconnected (contradiction)
- if degree of $v$ is 1, remove $v$ and it's edge to obtain a new connected graph $H$ with $m$ vertices and less than $m-1$ edges, a contradiction to the *inductive hypothesis*.

# Trees
A *simple undirected* graph $G$ is a **tree** if
- $G$ is *connected*
- $G$ contains *no cycles*

## Theorem
Any two of the following implies the third
- $G$ is connected
- $G$ contains no cycles
- $G$ has $n-1$ edges

## Rooted Tree
Choose a root node $r$ and orient edges away from $r$.
- Models a *hierarchical structure*
- Like a tree data structure