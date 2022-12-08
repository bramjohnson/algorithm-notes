# Breadth-First Search
"Find neighbors of neighbors"
$L_0$ = root, $L_1$ = neighbors of $L_0$, $L_2$ = neighbors of $L_1$, etc.

## Pseudocode
```
// Graph G and root node r
BFS(G, r):
	Let found[v] = false
	Let found[s] = true
	Let layer[v] = infty, layer[s] = 0
	Let i = 0
	Let L(0) = [s]
	Let T = empty 

	While (L(i) is not empty): // O (n + m)
		new L(i+1)
		For (u in L(i)): // Total of O(n)
			For (u,v in E): // Total of O(m)
				If (found[v] = false):
					found[v] = true
					layer[v] = i+1
					add (u,v) to T
					Add v to L(i+1)
		i += 1
```
The *layer* represents how far it is from the root node.
Total run time of $O(n+m)$

# 2-Coloring (Bipartiteness)
Graph of students who don't like each other. Want to make two teams of *red* and *blue*, where no pair in those sets don't like each other.

Graph $G$ is *bipartite* if $V$ can be split into two sets $L$ and $R$, such that all edges (u, v) in $E$ go between $L$ and $R$.

## Idea 1
1. BFS the graph, coloring nodes as you find them
2. Color nodes in layer $i$ blue if $i$ is *odd* and red if $i$ is *even* 

This approach doesn't work because a cycle of odd number of edges will fail. It also fails on edges with 2 nodes on the same layer.

## Key Fact
If $G$ contains a cycle of odd length, then $G$ is *not* bipartite (2-colorable). The final colored node will need to be neither *red* nor *blue*, which is impossible for 2 colors.

## Claim
If BFS did not 2-color the graph, the graph is not bipartite.

**Proof**: if BFS fails, then $G$ contains an odd cycle.
- Let $A$ be the closest common ancestor to some node $u$ and $v$ who have an edge.
- Dist($A$, $u$) == Dist($A$, $v$) == $x$.
- This means that there is a cycle from $A \gets u$, $u \gets v$, and $v \gets A$. Creating a cycle of length $x$, and by definition, $2x + 1$ is always *odd*.
- Therefore, we create a cycle of *odd* length, meaning BFS fails.