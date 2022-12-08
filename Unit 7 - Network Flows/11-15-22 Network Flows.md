# Flow Networks
Directed graph $G = (V, E)$.
Two special nodes: *source* $s$ and *sink* $t$.
Edge *capacities* $c(e)$.

## Flows
An $s$ to $t$ *flow* is a function $f(e)$ such that:
- For every $e \in E$ and $0 \leq f(e) \leq c(e)$ (at most capacity)
- For every $v \in V$ the *in-flow* is the same as the *out-flow* (no negative)

The *value* of a flow is $val(f) = \sum_{e out} f(e)$

# Maximum Flow Problem
We want to find a $s$ to $t$ flow of *maximum value*.

# Cuts
An $s$ to $t$ *cut* is a partition of $(A,B) \in V$ where $s \in A$ and $t \in B$.
- $s$ is on the $A$ side of the cut and $t$ is on the $B$ side.

The *capacity* of a cut is the sum of all edges leaving $A$.

## Minimum Cut Problem
Given $G$ find a $s$ to $t$ cut of *minimum capacity*.

# Flows & Cuts
**Fact**: If $f$ is any $s$ to $t$ flow, and $(A,B)$ is any $s$ to $t$ cut, then the net flow across ($A,B$) is equal to the amount leaving $s$.
AKA: $f = outflow(A) - outflow(B)$

**Fact**: for any $s$ to $t$ flow $f$ and any cut ($A,B$), then $val(f) \leq cap(A,B)$ is true.
- $val(f) = \sum f_{out}(e) - \sum f_{in}(e)$
- $val(f) \leq \sum f_{out}(e)$
- $val(f) \leq \sum f_{in}(e)$
Therefore $val(f) \leq cap(A,B)$

## Statements
The max flow always has an edge $e$ leaving the source $s$ such that $f(e) = c(e)$ (*saturated*)
- False, edge leaving source has capacity 5 and other edge has capacity 3

The max flow always has an edge $e$ such that $f(e) = c(e)$ (*saturated*)
- True, If no edge is saturated, then we can always get more flow infinitely.

# Augmenting Paths
Given a network $G$, and a flow $f$, an **augmenting path** $P$ is a simple $s-t$ path where $f(e) < c(e)$ for every edge $e \in P$. Another way to think about it is *no edge* in the path is *saturated*.

# Greedy Max Flow
- Start with $f(e) = 0$ for all edges $\in E$.
- Find *augmenting path* $P$ and increase flow by max amount.
- Repeat until you get stuck.
Not yet a max flow, still some instances where there can be a larger flow. To fix this issue, we need to have a way of sending flow *backwards* [[11-15-22 Network Flows#Residual Graph |using Residual Graphs]].

## Residual Graph
Original edge $e$ has *flow* and *capacity*. Use graph made from greedy max flow.
- *Residual capacity* = $c(e) - f(e)$ (How much more flow we can put on the edge).
- *Residual edge* allows "undoing of flow". $e = (u,v)$ and $e^R = (v,u)$. $c(e^R) = f(e)$
- *Residual graph* replaces all edge capacities with residual edges. Make backwards edge if edge has flow > 0 on it. Make forwards edge if flow < capacity.

![Residual Graph](Resources/residual.png)

## Ford-Fulkerson
Start with $f(e) = 0$ for all edge $\in E$.
- Find an *augmenting path* $P$ in the *residual graph*.
- Repeat until you get stuck.
This will find our correct flow for max flow

- Let $G$ be a *residual graph*
- Let P be an augmenting path in the *residual graph*
- **FACT**: $f = Augment(G, P)$ is a valid flow
```
Augment(G, P):
	b = min capacity of edge in P
	for e in P:
		if (e is an original edge):
			f(e) = f(e) + b
		else:
			f(eR) = f(eR) - b
	return f
```
O(n)

```
FordFulkerson(G, s, t, c(e)):
	for e in E: f(e) = 0
	G is residual graph

	while (there is an s-t path P in G):
		f = Augment(G, P)
		update G
	return f
```
O(m) per loop

## Running Time
- Ford-Fulkerson: For *integer capacities* < $val(f)$, where $f$ is the max flow
- Can perform each *augmentation* step in $O(m)$ time
- Therefore, for integer capacities, FF runs in $O(m * val(f))$ time

## Correctness
*Theorem*: $f$ is a max $s-t$ flow, if and only if there is no *augmenting* $s-t$ path in $G$.
*Strong MaxFlow-MinCut Duality*: The value of the max $s-t$ flow equals the capacity of the min $s-t$ cut.

We will prove that the following are equivalnet for all $f$
1. There exists a cut ($A,B$) such that $val(f)$ = $c(A,B)$
2. Flow $f$ is a max flow
3. There is no augmenting path in $G$.

**2 -> 3**: Using contrapositive, if there is an augmenting path in $G$, then $f$ is not a max flow. Therefore we flipperoo.
**1 -> 2**: If $val(f) = c(A,B)$ then $f$ is a max flow.
**3 -> 1**: No Augmenting path in $G$, then there is a cut such that $val(f) = c(A,B)$
- $s$ is on $A$ side and $t$ is on $B$ side, therefore $A, B$ is a valid $s-t$ cut.

**FACT**: every graph with *integer capacities* have an *integer max flow*.
