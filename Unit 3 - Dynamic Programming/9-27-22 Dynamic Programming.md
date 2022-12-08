# Dynamic Programming
General algorithm/design paradigm for creating algorithms.

Express the optimal solution as a **recurrence**
- Identify a small number of subproblems

Efficiently solve for the *value* of the optimum
- Simple implementation is exponential time, but top-down and bottom-up are linear time.

Find the solution using the table of values.

## Recipe
1. Identify a set of subproblems
2. Relate subproblems to recurrence
3. Find efficient implementation of recurrence
4. Reconstruct the solution from the dynamic programming table

# Fibonacci Numbers
## Recursively
F(n) can be solved recursively:
```
Fib(n):
	If (n = 1): return 0
	If (n = 2): return 1
	Return Fib(n-1) + Fib(n-2)
```
$T(n) = T(n-1) + T(n-2) + 1$
$T(n) = 2F(n+1) - 1$
$T(n) = O(1.62^n)$

## Memoization
Store solutions to subproblems:
```
M <- empty array, M[1] <- 0, M[2] <- 1
Fib(n):
	If(M[n] is not empty): return M[n]
	If(M[n] is empty):
		M[n] <- Fib(n-1) + Fib(n-2)
		return M[n]
```
How many calls does Fib(n) make?
1 element = 2 recursive calls
therefore, the total is $2n = O(n)$

This method is **top down** (start at top of recursion, and go down)

## Memoization 2

```
M <- empty array, M[1] <- 0, M[2] <- 1
Fib(n):
	For i = 3, ..., n:
		M[n] <- M[n-1] + M[n-2]
	return M[n]
```
n-2 loops, therefore $O(n)$

This method is **bottom up** (iterative, start at base case)

# Weighted Interval Scheduling
How can we optimally schedule a resource (such as a classroom)?

**Input**: n intervals ($s_i, f_i$) each with value $v_i$.
- Assume intervals are sorted by time

**Output**: a compatible schedule S *maximizing* the total value of all intervals
- In a schedule, no interval overlaps
- The *total value* of S is the sum of the values.

## Recursive
Let $O$ be the *optimal* schedule.
- Case 1: Final interval (n-1) is not in $O$, then don't include it
- Case 2: Final interval (n-1) is in $O$, then include it and create a list where any conflicts with (n-1) are removed.

So, which case is better?
*Case 1*: Optimal solution for {1, n-1}
*Case 2*: Optimal solution for {1, ..., n}

Term: *OPT(i)* = value of the optimal solution, for the first *i* intervals

For this problem, *OPT(i)* = max(Opti(i-1), v_i + OPT(p(i)))
- v_i = current value
- p(i) = non-conflicting values

Example values:
| Node | Value | Conflicts     | p(n) Non-conflicts before it | 
| ---- | ----- | ------------- | ---------------------------- |
| 1    | 2     | 2, 4,         | 0                            |
| 2    | 4     | 1, 3, 4       | 0                            |
| 3    | 4     | 2, 4          | 1                            |
| 4    | 7     | 1, 2, 3, 5, 6 | 0                            |
| 5    | 2     | 4             | 3                            |
| 6    | 1     | 4, 5          | 3                            |

**Step 3**: find efficient algorithm

#### Recursive Solution
```
FindOPT(n):
	if(n = 0):
		return 0
	v = final value of n
	if (n = 1):
		return v
	return max(FindOPT(n-1), v + FindOPT(p(n)))
```
O(n^a)

## Dynamic Programming

#### Top Down
```
M <- empty, M[0] <- 0, M[1] <- v
FindOPT(n):
	if (M[n] is not empty): return M[n]
	M[n] <- max(FindOPT(n-1), v + FindOPT(p(n)))
	return M[n]
```
O(n)

M[6] = max(FindOPT(5), 1 + FindOPT(3)) = (8, 7) = 8
M[5] = max(FindOPT(4), 2 + FindOPT(3)) = (7, 8) = 8
M[4] = max(FindOPT(3), 7 + FindOPT(0)) = (6, 7) = 7
M[3] = max(FindOPT(2), 4 + FindOPT(1) = (4, 6) = 6
M[2] = max(FindOPT(1), 4 + FindOPT(0)) = (2, 4) = 4
M[1] = max(FindOPT(0), 2 + FindOPT(0)) = (0, 2) = 2
M[0] = 0

#### Bottom Up
```
FindOPT(n):
	M[0] <- 0, M[1] <- v
	for(i = 2, n):
		M[i] <- max(M[i-1], v + M[p(i)])
	return M[n]
```

M[0] = 0
M[1] = max(M[0], 2 + M[0]) = (0, 2) = 2
M[2] = max(M[1], 4 + M[0]) = (2, 4) = 4
M[3] = max(M[2], 4 + M[1]) = (4, 6) = 6
M[4] = max(M[3], 7 + M[0]) = (6, 7) = 7
M[5] = max(M[4], 2 + M[3]) = (7, 8) = 8
M[6] = max(M[5], 1 + M[3]) = (8, 7) = 8


**Step 4**: reconstruct the answer from DP table

```
FindSched(M, n):
	if (n = 0): return 0
	if (n = 1): return [1]
	elsif(v_n + M[p(n)] > M[n-1])
		return [n] + FindSched(M, p(n)) // Case 2
	else:
		return FindSched(M, n-1) // Case 1
```
O(n)

FindSched(M, 6) = FindSched(M, 5)
FindSched(M, 5) = [5] + FindSched(M, 3)
FindSched(M, 3) = [3] + FindSched(M, 1)
FindSched(M, 1) = [1]

= [5, 3, 1], and yes that is the optimal schedule.

#### Space Complexity
We need to store n values in the M array, therefore we have O(n) space complexity.