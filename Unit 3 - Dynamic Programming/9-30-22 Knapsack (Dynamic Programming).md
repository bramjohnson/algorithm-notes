# Knapsack
1. Identify set of subproblems (for knapsack, more than one variable)

**Problem Description**: You are given $n$ items for your knapsack. Each item has a value $v$ and a weight $w$. The knapsack has a capacity $T$.

The goal is to find the most valuable subset of item that fits in the knapsack (maximize for $v$).

*Example:*
Item Count: 5
Knapsack Capacity: 15

| Value | Weight | #   |
| ----- | ------ | --- |
| 2     | 2      | 1   |
| 1     | 1      | 2   |
| 10    | 4      | 3   |
| 2     | 1      | 4   |
| 4     | 12     | 5   | 

**argmax**: Find subset that maximizes $v$ of subset, such that the $w$ of the subset is $\leq T$
- Returns the subset

## Cases
$OPT(j, S)$ is the value of the optimal subset of items 0, ..., $j$ in a knapsack of size $S$.
1. the nth item is not in opt solution
$n$ is not in the solution, therefore, our optimal solution looks like $OPT(n-1, T)$

2. the nth item is in opt solution
$n$ is in the solution, therefore our optimal solution looks like $[n] \cup OPT(n-1, T - w_n)$

## Recurrence
**Recurrence**:
$OPT(j, S) = max(OPT(j - 1, S), OPT(j - 1, S - w_j))$
- If $S - w_j < 0$ do not call $OPT(j - 1, S - w_j)$ because $j$ cannot fit in the knapsack.

**Base Case**:
- If $S \leq 0$ return 0
- If $j = 0$ return $v_j$

## Bottom Up
```
FindOpt(n, T):
	M[0, S] = O
	M[j, 0] = 0

	for (j = 1, ..., n):
		for (S = 1, ..., T):
			if (j.weight > S): M[j, S] = M[j-1, S]
			else: M[j, S] = max(M[j-1, S], j.value + M[j-1, S-j.weight])
	return M[n, T]
```

#### Example
Input: T = 8, n = 3
- $w_1 = 2$, $v_1 = 4$
- $w_2 = 3$, $v_2 = 5$
- $w_3 = 5$, $v_3 = 8$

height = items, width = capacities
| --- | 0   | 1   | 2   | 3   | 4   | 5   | 6   | 7   | 8   |
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   | 0   |
| 1   | 0   | 0   | 4   | 4   | 4   | 4   | 4   | 4   | 4   |
| 2   | 0   | 0   | 4   | 5   | 5   | 9   | 9   | 9   | 9   |
| 3   | 0   | 0   | 4   | 5   | 5   | 9   | 9   | 12  | 13   |
Space/Time complexity: O(nT)

## Solution
Filling the knapsack

```
M contains solutions to subproblems
FindSol(M, n, T):
	if (n = 0 OR T = 0): return empty
	if (n.weight > T): return FindSol(M, n-1, T)
	else:
		if (M[n-1, T] > n.value + M[n-1, T-n.weight]): return FindSol(M, n-1, T)
		else: return {n} + FindSol(M, n-1, T-n.weight)
```
Worst case runtime: $O(n)$ because each call is decrementing $n$. The worst case is when the optimal solution is adding nothing to the bag.