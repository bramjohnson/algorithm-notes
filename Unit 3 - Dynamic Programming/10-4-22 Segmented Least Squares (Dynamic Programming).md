# Least Squares
Attempting to find the line of best fit with the lowest error. Imagine a graph with lots of points on it, where many points are in lines, but those lines are not aligned.

For *Segmented* Least Squares, we create segments where there is a good line of fit.

# Cost Least Squares
Each point is assigned a cost parameter. We want to partition the list into contiguous segments that minimize cost.

**Cost**: mC + Sum(i=1, m, error(Li, Si)). Where m = segments, and the sum represents the error in each segment

## Example
| Potential Segment | Optimal Line | Error |
| ----------------- | ------------ | ----- |
| A                 | y = 1        | 0     |
| B                 | y = 1        | 0     |
| C                 | y = 3        | 0     |
| A, B              | y = 1        | 0     |
| B, C              | y = 2x-3     | 0     |
| A, B, C           | y = x - 1/3  | 2/3   | 

**Possible Solutions**:
| Solution      | Cost     |
| ------------- | -------- |
| [A], [B], [C] | 3C       |
| [A, B], [C]   | 2C       |
| [A], [B, C]   | 2C       |
| [A, B, C]     | 1C + 2/3 | 
If C $\geq 2/3$, then [A, B, C] is optimal
If C < $2/3$, then either of the 2C are optimal

## Pre-Processing
To help our algorithm, we can compute the error for each possible segment. This will take O(n^2) time (I don't know why). 

## Dynamic Programming
Let $O_j$ be the *optimal* solution for all of the points up to $j$.
Therefore, the final segment in $O_j$ is $p_i , ..., p_j$. We know it ends at $j$.

If the final segment is $p_n, ..., p_j$, then the optimal solution is $O_n = p_i, ..., p_n$
**Optimal Solution**: $= O_{i - 1}$

**Case i**: final segment is $p_i, ..., p_j$
- Optimal solution is $L_{i,j} \cup OPT(i-1)$
- Can use any $i$ from $i, ..., j$

**Total Cost**: 1 + 2 + 3
1. Error for $i, ..., j$
2. Cost of $i, ..., j$
3. $OPT(i - 1)$

## Recurrence
$OPT(j) = min(err + C + OPT(i - 1))$ for $i, ..., j$

**Base Case**:
- $OPT(0) = 0$
- $OPT(1) = OPT(2) = C$

#### Take 1
```
FindOPT(n):
	if (n = 0): return 0
	if (n == 1, 2) return C
	return for(1, i, n): min(err + C + FindOPT(i - 1))
```
This is not dynamic programming because there is no memoization!

#### Take 2 (Top-down)
```
M[0] = 0
M[1] = C
M[2] = C
FindOPT(n):
	if (M[n] exists): return M[n]
	M[n] = for(1, i, n): min(err + C + FindOPT(i - 1))
	return M[n]
```
Have to fill O(n) elements.
To fill them, we will make O(n) recursive calls
Therefore, we have a runtime of O(n^2)

#### Take 3 (Bottom-UP)
```
FindOPT(n):
	M[0] = 0
	M[1] = C
	M[2] = C
	for (j = 3, ..., n):
		M[j] = for(1, i, n): min(err + C + FindOPT(i - 1))
	return M[n]
```
Two for loops, therefore O(n^2) time.

## Finding Solution
```
if x == argmin(1 <= i <= n)(err + C + M[i-1]):
	then [px, ..., pn] is one segment in the solution
	n = x - 1
```
**argmin** returns *i* corresponding to the minimum value
If n = 26, argmin will return 17 if the value at 17 is the minimum value
The runtime for **argmin** is $O(n)$.

Full Pseudocode
```
FindSol(M, n):
	if (n = 0): return empty
	if (n = 1): return {1}
	if (n = 2): return {1, 2}
	Let x = argmin(1, ..., n)(error + C + M[i-1])
	return {x, ..., n} + FindSol(M, x-1)
```
Runtime = $O(n^2)$
Space = $O(n^2)$ (dominated by error, which has worst case of $n^2$).

# Segmented Least Square V2
Multiway choices + 2D table

**Input**: n data point where number of segments must be less than $k$
**Output**: partition of points into $\leq k$ continuous segments
**Cost**: Sum(err(L, S))

# Optimal Solution
$O_j$ is the *optimal* solution for points\[1, …, j\]. The final segment in $O_j$ must end be points\[i, …, j\] where $1 \leq i \leq k$.

Let $OPT(j, L)$ be the optimal solution for points\[1, ..., j\] using $\leq L$ segments.
**Case i**: final segment is \[i, ..., j\]

# Recurrence
**Recurrence**: $OPT(j, L) = min(err + OPT(i - 1, L-1))$

**Base Case**:
- $OPT(0, L) = 0$
- $OPT(j, 0) = \infty$  (not allowed!)

## Bottom Up
```
FindOPT(n, k):
	M[0, L] = 0, M[j, 0] = infty
	for (L = 1, ..., k):
		for (j = 1, ..., n):
			M[j, L] = min(1, i, j)(err + M[i-1, L-1])
```

# Finding Solution
```
FindSol(M,n,k):
	if (n = 0): return empty
	if (n = 1): return {1}
	Let x = argmin(1, ..., n)(err + M[i-1, k-1])
	return {x, ..., n} + FindSol(M, x-1, k-1)
```

**Time Complexity**: $O(nk * n)$
**Space Complexity**: $O(n^2)$