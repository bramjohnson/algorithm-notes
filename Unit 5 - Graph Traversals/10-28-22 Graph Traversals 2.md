# Connected Components
Given an undirected graph $G$ split into connected components. 

## Algorithm
- Pick a node $v$.
- Use DFS to find all nodes reachable by $v$.
- Labels those as one connected component.
- Repeat until all nodes are in one connected component.

```
CC(G):
	let comp[1:n] = c <- 1

	for (u = 1, ..., n):
		if (comp[u] = null):
			run DFS(G, u) O(nodes reachable from u + edges reachable from u)
			let comp[v] = c for every v in DF
			let c += 1

	Return comp[1:n]
```

Each connected component will be seen exactly one time. Therefore our inner DFS will only result in $O(n+m)$.
Split graph into connected components in time $O(n+m)$

# Strongly Connected Components
Given a directed graph $G$, split it into strongly connected components. A strongly connected component is a set of nodes that can all reach each other.

## Observation
SCC(s) is all nodes $v$ in $V$ such that $v$ is reachable from $s$ and $s$ is reachable from $v$.

## Algorithm
```
SCC_Slow():
	G_R = G with all edges reversed

	comp[1:n] = empty, c = 1
	
	for (u = 1, ..., n):
		if (comp[u] = empty):
			S = set of nodes found by DFS(G, u)
			T = set of nodes found by DFS(G_R, u)
			label S and T with c
			c += 1
	
	return comp
```

If each node is it's own SCC, then we cannot guarantee that the runtime will be $O(n+m)$. In other words, we cannot say that DFS of each node will reveal all it's edges.

## Algorithm 2
We can make the observation that each SCC is a cycle. Therefore, connect all our SCCs forms a DAG (acyclic). If we had a cycle, then we wouldn't have a SCC.

DFS from any node in a *sink component* finds that component, where a **sink component** is a SCC w/ out-degree of 0. If a node is a *sink component* then any node it can reach must be in its SCC.

We can find a sink component by finding the node with the largest finish time in the reverse of the graph.

```
SCC(G):
	G_R = G with all edges reversed
	DFS(G_R) computes finish times of reverse graph
	comp[1:n] = empty, c = 1
	for (u in reverse order of finish time): // Always DFS from node in sink component
		if (comp[u] = empty):
			S = set of nodes found by DFS(u) in G
			for v in S comp[v] = c
			c += 1
			
	return comp
```
This approach removes the duplicate nodes found in DFS from the previous technique. Each edge and node are visited exactly once due to the sink component. 

If you want to find the SCC of a graph, you can do so in $O(n+m)$ 
"Use the SCC algorithm used in class" - can write on HW