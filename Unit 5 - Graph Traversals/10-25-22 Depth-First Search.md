# Adjacency Matrix
An adjacency matrix of a graph $G$ is a 2D table of size $VxV$.
*Space Complexity*: $O(n^2)$
*Lookup Time*: $O(1)$
*List Neighbors*: $O(n)$ (how many in the row/col are not 0)

# Adjacency List
An adjacency list of vertices $v$ is a list of all *outgoing* connections from $v$ to $[u]$.
*Space Complexity*: $O(n + m)$ where $n$ is the vertices and $m$ is all the outgoing connections.
*List Neighbors*: $O(degree(v))$.

## Directed
Two adjacency lists of outgoing and incoming:
*Space Complexity*: $O(n+m)$
*List Out/In*: $O(deg(v))$

# Graph Traversal
**Problem**: is there a path from $s$ to $t$.
- We can explore all nodes reachable from $s$ in the worst case.

There are two different search techniques to find this solution:
- Depth-First Search: follows a path until getting stuck, and then gets back
- Breadth-First Search: explore all immediate paths and then continue.

# Depth First Search
## Psuedocode
```
G = (V, E) is a graph
discocvered[u] = 0 for all u
parent[u] = nothing for all u

// Start Node is u
DFS(u):
	discovered[u] = 1

	for (v in E[u]):
		if (discovered[v] = 0):
			parent[v] = u
			DFS(v)

```

## Directed Graphs
Each edge in $G$ has a type
- *Tree* edges are ones that discover new nodes
- *Forward* edges are ancestor to descendant
- *Back* edges are descendant to ancestor (implies cycle)
- *Cross* edges have no ancestor relation
![DFS Tree](/Resources/Pasted%20image%2020221025141922.png)

## Discovery and Finish Times
We want to create a table of vertices, discovery, and finish times
| Vertex | Discovery | Finish |
| ------ | --------- | ------ |
| u      | 1         | 8      |
| a      | 2         | 3      |
| b      | 3         | 4      |
| c      | 5         | 6       |

```
G = (V, E) is a graph
discocvered[u] = 0 for all u
parent[u] = nothing for all u
clock = 1

// Start Node is u
DFS(u):
	discovered[u] = 1
	d[u] = clock, clock++

	for (v in E[u]):
		if (discovered[v] = 0):
			parent[v] = u
			DFS(v)
	f[u] = clock, clock++

```
*Runtime*:
- $O(n)$ for initializing discovered
- $O(n)$ DFS calls for each node discovered
- $O(m)$ calls to for loop throughout algorithm
	- $degree(u)$ for each call; $sum(degree(u))$ is $m$ (number of edges)
Total: $O(n + m)$

![Finish Times](/Resources/Pasted%20image%2020221025143436.png)

### Classifying Edges
If $u$ is discovered first and there is a path from $u$ to $v$
- $d[u] < d[v] < f[v] < f[u]$
- *tree* or *forward* edge

If $u$ is discovered first and no path from $u$ to $v$
- $d[u] < f[u] < d[v] < f[v]$
- *back* edge

If $v$ is discovered first and there is a path from $v$ to $u$
- $d[v] < d[v] < f[u] < f[v]$
- *cross* edge

If $v$ is discovered first and no path from $v$ to $u$
- $d[v] < f[v] < d[u] < f[u]$
- Impossible

# Directed Acyclic Graph
**DAG** is a *directed* graph that has no *directed cycles*.
- Can be more complex than an undirected graph without cycles.

DAGs capture precedence relationships, where one thing must come before another.
- Edge $(u,v)$ indicates that $u$ needs to be completed before $v$ can be done
- In what order should the activities be completed?
	- Topological ordering
**Topological ordering** is a labeling of nodes from $v_1$ to $v_n$ so that all edges go "forwards". There are no back loops and following connections leads to a leaf.

## Degrees
The first node must have no *in-edges*. This means the first node always has a node of *in-degree* $0$.
- In any *DAG*, there is always a node with no incoming edges (*in-degree* of 0).
To prove this: consider a longest simple path in DAG $P$. Let $v$ be the first node in $P$.
- *Case 1* is that $v$ has an *in-edge* that is in $P$, which cannot happen because it causes a cycle.
- *Case 2* is that $v$ has an *in-edge* that is not on $P$, which cannot happen because then $P$ would not be the longest simple path.

## Existence of Topological Ordering
In any DAG, there is a node with *in-degree* 0. From this we can theorize that every DAG has a topological ordering. To prove this we will use induction.

### Base
H(1) there is no other node to have an in degree.

### Inductive Hypothesis
$H(k-1) \longrightarrow H(k)$. We need to show that every DAG with $k$ nodes has topological ordering.

### Inductive Step
Let $G$ be a DAG w/ $k$ nodes and $v$ be a node with *in-degree* 0. We know that $G - v$ has $k-1$ nodes and is a DAG. By our inductive hypothesis, $G - v$ has a topological ordering $O$. If node $v$ is placed at the start of $O$, we obtain a topological ordering of graph $G$ with $k$ nodes.

## DAGs and Back Edges
For any DAG $G$, in any DFS traversal of $G$, there is no back edge due to the definition of a DAG.

## Topological Ordering Algorithm
Repeatedly find node $u$ with zero *in-degree*.
- Place $u$ next in the order
- Remove $u$ and *out-edges*
- Update *in-degrees*
- $O(n+m)$ time

```
Z, Q = empty stack, queue
for u in V: // O(n) (n = # of nodes) (m = # of edges)
	compute in-degree(u) // O(m) for ALL u... O(in-degree(u)) for EACH u...
	if in-degree(u) = 0:
		push u into Z
// O(n + m)

While Z is not empty: // O(n)
	pop u from Z
	insert u into Q
	for edge (u,v): // O(m) TOTAL
		decrement in-degree(v)
		if in-degree(v) = 0
			push v into Z

Return Q
```

Can compute a topological ordering in $O(n + m)$ time using DFS.
