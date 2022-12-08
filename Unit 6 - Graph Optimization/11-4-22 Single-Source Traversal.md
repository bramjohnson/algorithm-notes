# Weighted Graphs
A weighted graph has edges with **costs** or weights between nodes. This may mean there are longer paths that have lower *cost* than shorter paths.

## Shortest Path
In weighted graphs, the *length* of path $P$ is the sum of it's edge weights.
The *distance* $d(s, t)$ is the length of the shortest path from $s$ to $t$.
**Shortest Path** finds the shortest path from $s$ to $t$.
**Single Source Shortest Paths** finds the shortest path from $s$ to *every* other node.
**All-Pairs Shortest Paths** finds the shortest path between *every pair* of nodes in the graph.

## Structure of Shortest Paths
If there is an edge $(u,v)$, then the $d(s, v) \leq d(s,u) + w(u,v)$ for every node $s \in V$. In other words, a less direct path going through $s, u, v$ is greater than or equal to in length the path $s, v$, given the fact that $u,v$ exists.

If there is an edge $(u,v)$ and $d(s,v) = d(s,u) + w(u,v)$, then there is a shortest $s \rightarrow v$ path ending with $(u,v)$.

# Dijkstra's Algorithm
Shortest paths algorithm. Modified version of BFS for non-negatively weighted graphs.

## Explanation
1. Maintain a set $X$ of explored nodes.
2. Maintain an upper bound on distance for all *unexplored* nodes.
	- If $u$ is explored, then we know the distance from $s$ to $u$. Once we explore a node, we know it's distance is *correct*.
	- If $u$ is explored, and $(u,v)$ is an edge, then we know the distance from $(s,v)$ is $(s,u) + (u, v)$.
3. Explore the "closest" unexplored node
4. Repeat

**Closest** node is the next *unexplored* node with the smallest upper bound on it's distance.

## Demo
![Demo](/Resources/Pasted%20image%2020221101145911.png)

| ---  | s   | B        | C        | D        | E        |
| ---- | --- | -------- | -------- | -------- | -------- |
| d(-) | 0   | $\infty$ | $\infty$ | $\infty$ | $\infty$ |
| d(s) | 0   | 10       | 3        | $\infty$ | $\infty$ |
| d(C) | 0   | 7        | 3        | 11       | 5        |
| d(E) | 0   | 7        | 3        | 11       | 5        |
| d(B) | 0   | 7        | 3        | 11       | 5        |
| d(D) | 0   | 7        | 3        | 11       | 5        |
The bottom row becomes our answer.

## Correctness
### Base 1
Initially $d_0(s)$ is the correct distance $d(s,s)$.
This is true for all non-negative weight graphs, because 0 is the minimum distance possible.

### Base 2
Before we explore the second node $v$, $d_1(v)$ is the correct distance $d(s,v)$.
This is true, because out of all the out neighbors of $s$, the smallest edge is the minimum distance to that node $v$.

### Invariant:
Before we explore the $k$th node $v$, $d_{k-1}(v)$ is correct (tight).

### Strong Induction
Assume invariant is true for $i < k$.
NTS: invariant is true for $i = k$

### Proof
$X$ is the set of explored nodes.
$\exists$ a $s, v$ path $P$ of length $d_{k-1}(v) = d(s,u) + w(u,v)$
- Consider any other $s,v$ path $P^1$.
- We know that $P^1$ leaves $X$ on edge $y,z$.
	- $y \in X$ and $z$ not $\in X$.
- $P^1$ then is $s,v,y,z$.

We NTS that the length of $P^1$ is longer than $P$.
- $P^1 = d(s,y) + w(y,z) + l(z,v)$ $\geq d_{k-1}(z)$ $\geq d_{k-1}(v)$ $= l(P)$
Therefore, $P^1$ is $\geq P$.

## Using HashMaps
The time complexity is $O(n^2 + m)$ for each node and finding min for each call.

## Using Heaps
The time complexity is $O(logn(n+m))$. Generally assume $m$ is larger than $n$, so we can upper bound to $O(mlogn)$.

# Priority Queues and Heaps
Priority queue is the concept, heap is the data structure. We need to key value pairs in a sorted way. Supports the following operations:
- Insert(Q, k, v): add a new key-value pair // O(logn)
- Lookup(Q, k): return the value of some key // O(1)
- ExtractMin(Q): return key with smallest value // O(logn)
- DecreaseKey(Q, k, v): reduce the value of some key // O(logn)

## Heap
Organize key-value pairs as a complete binary. If a is the parent of b, then val(a) $\leq$ val(b).

### ExtractMin
Remove root node (that is always the smallest). Take rightmost node from right remaining tree, and make that the root. Swap nodes until heap invariant is ensured.

### Down Heap
The process of swapping nodes until heap invariant is preserved.

## Array Heap
Implement a binary tree using a heap with an array. The root becomes the first index of the array, and you calculate left and right using 2i and 2i+1 respectively. The Parent of any child is floor(i/2).
