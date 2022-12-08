# Recursion Tree
## Exercise #1
$T(n) = 3T(n/2) + n^2$
$T(1) = 1$

			n
		n/2 n/2 n/2
	n/4 n/4 n/4 n/4 n/4

| Level | Nodes | Size  | Work         |
| ----- | ----- | ----- | ------------ |
| 0     | 1     | n     | cn^2         |
| 1     | 3     | n/2   | 3(n/2)^2     |
| 2     | 9     | n/4   | 9(n/4)^2     |
| 3     | 27    | n/8   | 27(n/8)^2    |
| i     | 3^i   | n/2^i | 3^i (n/2^i)^2 |
|       |       |       |              |
Work on level $i = 3^i (n^2/4^i) = (3/4)^i n^2$
Last Level: = $\lg n$
Total Work: = $n^2 * Sum(i=0, \lg n, (3/4)^i)$ = $O(n^2)$

## Exercise #2

## Handle Recurrences via Recursion Trees
Write the recurrence in a form so that you can apply it for different problem sizes.
Build the recurrence tree:
- Number of levels is the depth of the recursion
- For each level, find the work done at that level
- Look for patterns across work done per level
	- Same, decreasing, or increasing.

# Master Theorem
## Generic Divide/Conquer
Split into $a$ pieces of size $n/b$, and merge in time $O(n^d)$.
Takes the form of $T(n) = aT(n/b) + Cn^d$

#### Merge Sort
$a = 2$, $b = 2$, $d =1$

#### Karatsuba
$a = 3$, $b=2$, $d=1$

## Generic Recursion Tree
$T(n) = aT(n/b) + n^d$
$T(1) = 1$

| Level | Nodes | Size    | Work         |
| ----- | ----- | ------- | ------------ |
| 0     | 1     | n       |              |
| 1     | a     | n/b     | a(n/b)^d     |
| 2     | a^2   | (n/b^2) | a^2(n/b^2)^d |
| i     | a^i   | (n/b^i) | a^i(n/b^i)^d |
|       |       |         |              |
Work on level i = $a^i(n/b^i)^d$
Last Level: $log_b n$
Total Work: $n^d Sum(i=0, log_b n, (a/b^d)^i)$
This leads us to the Master Theorem.

## Master Theorem Proof
For any recurrence of the form $T(n) = aT(n/b) + Cn^d$

If $a/b^d > 1$
Then $n^d * c(a/b^d)^{log_b n}$
$= c(n^{log_b a})$
$= O(n^{log_b a})$

If $a/b^d = 1$
Then $n^d * (log_b n + 1)$
$= n^dlog_b n + n^d$
$= \Theta(n^dlog_b n)$

If $a/b^d < 1$
Then $n^d c$
$= \Theta(n^d)$

## Exercises
$T(n) = 16T(n/4) + Cn^2$
- $a = 16$, $b = 4$, $d = 2$
- $16/4^2 = 1$
- Therefore, $= \Theta(n^2 log_4 n)$
$T(n) = 21T(n/5) + Cn^2$
- $a = 21$, $b = 5$, $d = 2$
- $21/25 < 1$
- Therefore, $= \Theta(n^2)$
$T(n) = 2T(n/2) + Cn^0$
- $a = 2$, $b = 2$, $d = 0$
- $2/1 > 1$
- Therefore, $= \Theta(n^{log_2 2})$
$T(n) = 1T(n/2) + Cn^2$
- $a = 1$, $b = 2$, $d = 0$
- $1/1 = 1$
- Therefore, $= \Theta(n^0log_2 n) = \Theta(log_2 n)$

# Selection Sort
## Honse Race
You have 25 horses and want to find the fastest 3 horses. You can race 5 at a time and host 7 races.

#### Solution
Race 1-5: random 5 unique horses; race all 25 horses
Race 6: top horse from each race.
- Eliminate each horse in slowest 2 groups
- Eliminate every other horse except fastest in 3rd slowest group
- Eliminate every other horse except fastest and second fastest in 2nd fastest group
- Keep top 3 horses from winning group
- The winning horse is the fastest.
Race 7: race the remaining 5 horses to find order.

## Reduce and Conquer
Similar strategy to binary search (throw out part of array every time).

**Strategy**:
- Define a pivot element
- Partition into two parts around this element
	- All smaller elements to left of pivot, all larger elements to right of pivot
- Position of the pivot becomes the k-th smallest element.
- Throw out unneeded side of array

#### Psuedocode
```
Select(A[1:n], k):
	If(n=1) return A[1]

	Choose a pivot p = A[1]
	Partition around the pivot, let r = indexOf(A,p)

	If (k == r) return A[r]
	If (k < r) return Select(A[1:r-1], k)
	If (k > r) return Select(A[r+1:n], k-r)
```
Problem: If the list is already sorted, it takes $O(n^2)$ time to solve.
Solution: Pivot should always be the median

## Median of Medians
```
MOM(A[1:n]):
	Let m <- [n/5]
	For i = 1, m:
		Meds[i] = median{A[5i-4], A[5i-3], ..., A[5i]}
	Let p <- Select(Meds[1:m], [m/2])
```
Find the median of groups of five, and then find the median of that.
Finding the Median of 5 elements are constant.
Finding the Median is O(n) time.

```
MOMSelect(A[1:n], k):
	If (n <= 25): sort & return A[k]

	Let p = MOM(A)
	Partition around the pivot, let p = A[r]

	If(k = r): return A[r]
	If(k < r): return MOMSelect(A[1:r-1], k)
	if(k > r): return MOMSelect(A[r+1:n], k-r)
```

#### Runtime
`MOM(A)` - O(n) + `MOMSelect(n/5)`

Therefore, $T(n) = \frac{O(n) + T(n/5)}{Mom}$
$T(n) = T(7n/10) + T(n/5) + Cn$

*Longest level*: $(7/6)^in$
*Total Work*: Sum(i=0, $log_{10/7}n$, $(9/10)^i$) = $O(n)$
