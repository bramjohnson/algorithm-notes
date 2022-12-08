# Ford-Fulkerson
## Fattest Augmenting Path
We want to find the path with the **maximum-capacity**. Find *augmenting path* $P$ which has the largest value of $min(c(e))$ where $e \in P$. Find the path that maximizes the min weight of the path.

### Correctness
$f^1$ is a max flow with value $v^1 = val(f^1)$. $P$ is a fattest augmenting $s-t$ path with capacity $B$. 
We want to show that $B \geq v^1 / m$.

Number of augmenting paths available is $\leq m$, therefore it is true.

$v^1(1 - 1/m)^i <  1$
$i = m\ln(v^1)$
Therefore $v^1 * 1/(e^{\ln{(v^1)}})$
Therefore we have $O(m\ln(v^1))$
Total running time $O(m^2\ln(n)\ln(v^1))$

### Compared to Arbitrary Paths
When choosing arbitrary paths, we have $\leq v$ paths. In maximum-capacity paths, we have $f$

## Shortest Augmenting path
Find shortest augmenting path in $O(m)$ time using BFS
For any capacities (nm/2) augmentations suffice. Overall running time $O(m^2n)$.

Max flow can be solved in $O(mn)$. Can use this fact for all assignments and exams.

## Need to KNow
- Augmenting Path
- FF (Flow)
- Residual grpah
- Cuts and Flows
- Max flows
- Min Cut

# Network Flows
## Reduction
A **reduction** is an efficient algorithm that solves problem $B$, using calls to a function that solves problem $A$. Example: applying network flows to other problems.

What is a *problem*? It has a set of legal inputs $X$, and a set $A(x)$ of legal outputs for each $x$ in $X$.

### Formula
1. Input $x$ for problem $B$
2. Input $u$ for problem $A$
3. Solve $A$
4. Output $v$ in $A(u)$ for problem $A$
5. Output $y$ in $B(x)$ for problem $B$

- One function takes input of B and returns input of A
- One function solves A
- One function takes output of A and returns output of B

### Median
Find the median in an unsorted list
- Sort list in problem A
- Return middle element as answer

## Bipartite Matching
**INPUT**: bipartite graph $G$
**OUTPUT**: maximum cardinality matching.
- a matching is a set of edges such that every node $v$ is an endpoint of at most one edge in $M$.
This models any problem where one type of object is assigned to another type:
- doctors to hospitals
- jobs to processors

### Process
Undirected unweighted graph $A$ becomes direct graph with capacities $B$.
1. Transform the input
	- Given $A$ produce $B$
	- orient edges from Left to Right
	- Add a node s with edges from s to every node in L
	- add a node t with edges from t to every node in R
	- Set all capacities to 1

### Correctness
Each node in L has 1 edge of capacity 1 coming in from the source node $s$. Therefore each node has at most 1 unit of flow. Each node coming out of R can only have 1 unit of flow, therefore it can only have 1 incoming edge. Therefore, we must have a matching because there is only 1 unit of flow coming in.

$A$ has a matching cardinality $k$ if and only if $B$ has an s-t flow of value $k$. 
1. If we have a matching of cardinality $k$, then we can place 1 unit of flow on every edge in the matching. Ensure conservation is conserved when we add 1 unit of flow on each edge.
2. Proved above.

### Running Time
Populating $B$ takes $O(n+2m)$
Maxflow takes $O(nm)$
Producing $A$ using $B$ $O(m)$
$O(nm)$

## Edge Disjoint
**INPUT**: Directed graph
**OUTPUT**: Set of edge-disjoint paths (if paths do not share any edges)
A set of $k$ disjoint paths means we can tolerate k-1 edge failures.