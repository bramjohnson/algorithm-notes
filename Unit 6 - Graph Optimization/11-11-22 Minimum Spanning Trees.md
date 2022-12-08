# Minimum Spanning Tree
Build the cheapest, connected graph.
**INPUT**: Weighted graph $G$ that is *undirected* and *connected*. All edge weights are *distinct*.
**OUTPUT**: **Spanning tree** $T$ of *minimum cost*
- *Spanning Tree*: subset that has some edges from $G$ that is *connected* and *acyclic*.

## Cut Property
A **cut** is a subset of nodes $S$.
The **cutset** are edges w/ 1 endpoint in the cut.

**Cut Property**: Let $S$ be a *cut*. Let $e$ be the minimum weight edge cut by $S$. The *MST* must include $e$.
This edge $e$ is called a **safe edge**.

**Case 1**: deg(u) or deg(v) = 1
- $e$ must be in $T$
**Case 2**: deg(u) and deg(v) > 1
- Assume $T$ does not contain $e$
- There exists another edge $f$ in cutset
	- $f$ is the only other s-v path in $T$
- NTS: w(e) < w(f) therefore cost(T - f + e) < cost(T)
	- Still a spanning tree (acyclic and connected)

## Cycle Property
Let $C$ be a cycle. The max cost edge in this cycle is a *useless edge* and can be removed.

**Fact**: a cycle and cutset intersect in an even number of edges, because you must enter and leave a cutset to complete the cycle

### Proof
Let $C$ be a cycle and $f$ be the max weight edge in $C$. 
*NTS*: The MST $T^1$ does not contain $f$. Proof by contradiction.
- Assume $T^1$ contains $f$
- $T^1$ - $f$ is disconnected.
- Let $S$ be a connected component in $T^1 - f$, and $f \in cutset(S)$
- $cutset(S)$ intersects $C$ in an even # of edges $\geq 2 \in$ edge $e$, which is in $cutset(S)$ and in $C$.
- $w(e) < w(f)$, therefore $cost(T^1 - f + e) < cost (T)$
- Still a spanning tree

### Assumptions
- If $e$ is the edge with the smallest weight, then $e$ is always in the MST $T$.
- If $e$ is the edge with the largest weight, then $e$ may be in the MST $T$.

# Prim's Algorithm
Let $T$ be empty. Let $s$ be some arbitrary node and $S =$ {s}.
Repeat until $S = V$:
- Find the *cheapest* edge $e = (u,v)$ cut by $S$. Add $e$ to $T$ and add $v$ to $S$.

```
Prim(G=(V, E, w(E))):
	Let Q = priority queue storing V: // Total: O(nlogn)
		value[v] = inf, last[v] = empty // O(logn)
		value[s] = 0 for some arbitrary s
	while (Q not empty): // Total: O(nlogn)
		u = ExtractMin(Q)
		for each v in A[u]:
			if v in Q and w(u,v) < value[v]: // Total: O(mlogn)
				DecreaseKey(v, w(u,v))
				last[v] = u
	T = {(1, last[1]), ..., (n, last[n])} (excluding s)
	return T
```
*Time Complexity*: $O(mlogn)$

# Kruskal's Algorithm
Let $T$ be empty. For each edge $e$ in ascending order of weight:
- Add $e$ if it decreases the number of connected components (they are already ordering).

## Union-Find
Group items into components so that we can efficiently perform two operations:
- $Find(u)$: lookup which component contains $u$
- $Union(u,v)$: merge connected components of $u, v$

We can implement *Union-Find* so that:
- *Find* takes $O(1)$ time
- $k$ *Union* operations take $O(klogk)$ time.

### Fast Union-Find
Use array for current component of each vertex and *linked-list* for items in each component. Keep size of each component consistent (always union smaller onto larger).

1. After $k$ unions, only $O(k)$ items have changed component.
2. Largest component has size $O(k)$
3. Every time an item changes component, its new component is $O(2k)$ the size of its old component.
4. No item changed components more than $O(logk)$.
5. Total time: $O(klogk)$

## Informal Algorithm
- Time to sort: $O(mlogn)$
- Time to test edges: $O(1)$ per test
- Time to add edges: $O(m)$

# Prim's vs Kruskal's
Both take $O(mlog(n))$ time.