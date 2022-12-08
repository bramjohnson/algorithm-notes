# Bellman-Ford
Algorithm to determine shortest path from one source with *negative* edge lengths. One exception is there is no *negative* length cycles (that would create a way to get infinitely small weights).

## Dynamic Programming
**Subproblems**: Let OPT($v$) be the length of the shortest path from $s$ to $v$.
- For every $v$, the shortest path makes some final hop ($u$, $v$)
Case $u$: the final hop is ($u, v$)
- OPT($v$) = $OPT(u) + w(u,v)$
*Recurrence*: $OPT(v)$ = $min(u \in v)(OPT(u) + w(u,v))$
*Base*: $OPT(s) = 0$

Let $j$ be max *hops* we are allowed (max *edges*). This is at most $n -1$ edges.
Then our recurrence becomes:
*Recurrence*: $OPT(v,j)$ = $min(min(u,v \in inneighbors)(OPT(u,j-1) + w(u,v)), OPT(v,j-1))$
*Base*: $OPT(s, 0) = 0$ and $OPT(v, 0) = \infty$

## Finding Paths
$OPT(v,j)$ is the length of the shortest s-v path. $P(v,j)$ is the last hop on the shortest path.

*Case 1*: Skip edge.
$P(v,j) =$ if $OPT(v,j) = OPT(v, j-1)$ then $P(v, j-1)$.
*Case 2*: Include edge
$P(v,j) =$ if $OPT(v,j) = OPT(u,j-1) + w(u,v)$ then $P(v,j) = u$.

## Logistics
Algorithm uses $O(n^2)$ space to make a table, where width is number of hops, and height is the current edge. Fill in minimum path using $j$ hops from starting node $s$.

![Bellman-Ford](/Resources/Pasted%20image%2020221108141830.png|300)

|     | 0        | 1        | 2   | 3   | 4   |
| --- | -------- | -------- | --- | --- | --- |
| s   | 0        | 0        | 0   | 0   | 0   |
| b   | $\infty$ | -1       | -1  |  -1   |  -1   |
| c   | $\infty$ | 4        | 2   |   2  |   2  |
| d   | $\infty$ | $\infty$ | 1   |  -2   |  -2   |
| e   | $\infty$ | $\infty$ | 1   |  1   |   1  |

## Pseudocode
```
ShortestPath(G, s):
	foreach v in V:
		D[v,0] = inf
		P[v,0] = empty
	D[s,0] = 0

	for i = 1 to n-1 // O(n)
		foreach v in V: // O(n), when combined with above total = O(n^2)
			D[v,i] = D[v,i-1]
			P[v,i] = P[v,i-1]
			foreach u,v in E: // Total: O(m)
				if (D[u,i-1] + w[u,v] < D[v,i]):
					D[v,i] = D[u,i-1] + w[u,v]
					P[v,i] = u
```
*Time Complexity*: $O(n^2 + nm) = O(nm)$.
- $nm$ is bigger than $n^2$ because $m > n$.
*Space Complexity*: $O(n^2 + m)$
- Can be reduced to $O(n)$ if we only use one array instead of a table to keep track of previous values.

# Negative Cycle Detection
## Algorithm
- Pick a starting node $s$
- Run Bellman-Ford for $n$ iterations
- Check if $OPT(v,n) < OPT(v,n-1)$ for some $v \in V$
	- If no, then there are no negative cycles
	- If yes, the shortest s-v path contains a negative cycle.

# Optimized Bellman-Ford
```
Efficient-Shortest-Path(G,s):
	foreach v in V:
		D[v,0] = inf
		P[v,0] = empty
	D[s,0] = 0

	for i = 1 to n-1 // O(n)
		foreach u,v in E where D[u] changed during last iteration
			D[v,i] = D[v,i-1]
			P[v,i] = P[v,i-1]
			if (D[u,i-1] + w[u,v] < D[v,i]):
				D[v,i] = D[u,i-1] + w[u,v]
				P[v,i] = u
				if (i == n) return NEGATIVE CYCLE
			if (no D[u] changed) return (D,P)

```
$O(n+m)logn$
