# Problem
**INPUT**: Two strings
**OUTPUT**: Longest common subsequence of x and y

# Recurrence
*Question*: Are the last symbols of x and y in the subsequence?

**Possibility 1**: $x_n = y_m$
- $x_n$ and $y_m$ are both in the LCS
**Possibility 2**: $x_n \neq y_m$
- $x_n$ is not in the LCS
- $y_m$ is not in the LCS

## Recurrence
$LCS(i, j) =$ longest common subsequence of string $x$ with length $i$ and string $y$ with length $j$.

```
LCS(i, j):
	if xi = yj:
		return LCS(i - 1, j - i) + 1
	return max(LCS(i-1, j), LCS(i, j-1))
```

## Base Cases
$LCS(i, 0) = 0$
$LCS(0, j) = 0$

# Bottom-Up
```
FindOPT(n, m):
	M[i, 0] = 0
	M[0, j] = 0
	
	for (i = 1, ..., n):
		for (j = 1, ..., m):
			if (xi = yj):
				M[i,j] = 1 + M[i-1, j-1]
			else
				M[i,j] = max(M[i-1, j], M[i, j-1])
	
	return M[n,m]
```
**Time Complexity**: $O(nm)$
**Space Complexity**: $O(nm)$