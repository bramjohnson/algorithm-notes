# Divide & Conquer
Example: Mergesort! - Split into smaller subproblems; recursive.

## Mergesort
Merge(L, R): Sort lists L & R into list A and return.
- Runtime of Cn (constant * n) - constant time

Mergesort(A): Sort list A by splitting into L & R; Split, Sort, Merge, Return.

#### Finding Runtime
$T(n)$ = "runtime of Mergesort on array of size `n`"

```
Mergesort(A){
	if (len(A) <= 1) { return A }

	let L <- A[1:mid];
	let R <- A[mid:len(A)]

	let L <- Mergesort(L)
	let R <- Mergesort(R)

	let A <- Merge(L, R)

	return A
}
```

**T(n)**
- Sort L - $T(n/2)$
- Sort R - $T(n/2)$
- Merge(L, R) - $Cn$
- $= 2T(n/2) + Cn$

**Base Case:**
T(1) = C

#### Recursion Tree
			n
		n/2   n/2
	n/4           n/4

| Levels | Nodes | Size    | Work     |
| ------ | ----- | ------- | -------- |
| 0      | 1     | n       | cn       |
| 1      | 2     | n/2     | $2*cn/2$ |
| 2      | 4     | n/4     | cn       |
| i      | $2^i$ | n/$2^i$ | cn       |
Last level = $log_2(n)$

Work:
$Sum(i=0, log_2(n), cn$) = $cn(log_2(n) + 1)$

#### Summary
Sorting a list of n numbers in $Cn * log_2(n) *2n$

# Asymptotic Analysis
Usually do not have enough information to precisely predict running time of an algorithm. Also, different computers run programs at different speeds. Useful for comparing algorithms; care about how the algorithm scales. Especially critical in applications with emergence of big data.
**How does the running time grow as the size of the input grows?**

## Big-O Notation
$f(n) = O(g(n))$ if there exists $c \in (0, infinity) \wedge n_0 \in \mathbb{N}$ such that $f(n) <= c * g(n)$ for every $n >= n_0$

#### Proving Big-O
**Finding limit of ratio**:
lim(n->infinity)(10n + 50/n^2) = 0 (does not diverge)
**Proving from first principles**: Finding values c & n0.
Strategy for $c$: sum up coefficients of f(n)
Strategy for $n_0$: figure it out.

**Example**: $10(x+5) = O(x^2)$
- $f(n)$ *is* $O(g(n))$
- $g(n) = x^2$
- $10(x+5) <= c * x^2$ is *always true*
Once this the following are true, the Big-O notation is complete.

**Example**: $f(n) = 3n^2 + 10n + 5$ | $g(n) = n^2$
Using upper bounds:
- $3n^2 + 10n + 5 \to 3n^2 + 10n^2 + n^2 \to 14n^2$ where $n \geq 3$
- $3n^2 + 10n + 5 \leq 14n^2$ for $n \geq 3$

**Example**: $f(n) = 3n^2 + n$ = $O(n^2)$
- *Upper bound*: $3n^2 + n^2$
- $3n^2 + n \leq 4n^2$ for $n \geq 1$

**Example** $f(n) = 10n^4 = O(n^5)$
- $c = 10$
- $10n^4 <= 10n^5$ for $n \geq 1$

**Example** f(n) = $log_2(n) = O(log_2(sqrt(n)))$
- $g(n) = 1/2(log_2(n))$
- $c = 2$
- $log_2(n) \leq 2(1/2)(log_2(n))$ for $n \geq 1$

**Rank the Following**
- nlog2n
- $n^2$
- $100n$
- $3^{log2(n)}$

$100n$ = O($nlog2n$) = O($3^{log2(n))}$) = O($n^2$)

#### General Proof
Prove that if $f(n) = O(h(n))$ and $g(n) = O(h(n))$, then $f(n) + g(n) = O(h(n))$
- $f(n) \leq C_1 * h(n)$ - For all values $n \geq n_0'$
- $g(n) \leq C_2 * h(n)$ - For all values $n \geq n_0''$
- *Then:* $f(n) + g(n) \leq (C_1 * C_2) * h(n)$
- *Let* $C = (C_1 * C_2)$ is still a constant because a constant times a constant is a constant.
- *Therefore*: $f(n) + g(n) \leq O(h(n))$

## Big-O Rules
**Constant factors are ignored**
- Cn = O(n)
**Lower order terms dropped**
- $n^2 + n^3/2 + n = O(n^2)$
**Small exponents always Big-O of larger exponents
- $n^2 = O(n^3)$
**Any log is Big-O of any polynomial
- $log_2(n)^9 = O(n)$
**Any polynomial is Big-O of any exponential**
- $n^2 = O(2^n)$

## Big-Omega/Theta
**Big-Omega**: $f(n) \geq c * g(n)$
**Big-Theta**: $f(n) = g(n)$
- $f(n) = O(g(n))$ AND $f(n) = Omega(g(n))$
