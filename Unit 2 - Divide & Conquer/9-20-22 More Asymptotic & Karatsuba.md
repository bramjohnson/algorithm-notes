# Order of Growth
## Little-O
- Asymptotic version of $f(n) < g(n)$
- For **every** $c > 0$

**Example 1** - $3n^2 + n = o(n^2)$
- False; same order of growth
**Example 2** - $n^3 = o(n^2)$
- False; less order of growth
**Example 3** - $10n^4 = o(n^5)$
- True; greater order of growth
**Example 4** - $log2(n) = o(log2(sqrt(n)))$
- False; same order of growth.

## Little-Omega
- Asymptotic version of $f(n) > g(n)$
- Again for **every** $c>0$

# Order & Limits
If lim(f(n)/g(n)) =
- 0: $f(n) = o(g(n))$
- c > 0: $f(n) = Omega(g(n))$
- infinity: $f(n) = w(g(n))$

# Karatsuba's Algorithm
Addition: $2n + 1 = O(n)$
- $2n +1$ comes from adding $n$ numbers and carrying $n$ numbers and adding a new place.
Multiplication: $O(n^2)$
- $n^2$ multiplications + $n$ additions of $n$ digit numbers = $2n^2 = O(n^2)$

## Divide and Conquer: Multiplication
For multiplication of $ab * cd$:
$u = 10^{n/2} * a+b$ and $v = 10^{n/2}*c+d$

$u*v = 10^n ac + 10^{n/2}(ad + bc) + bd$
All addition is on sizes of $n/2$ multiplication.

$T(n) = 4 * T(n/2) + Cn$
but... this is still $O(n^2)$

#### Proof using Recursion Tree
				n
		  n/2  n/2  n/2  n/2
n/4 n/4 n/4 n/4 n/4 n/4 n/4 n/4 n/4 n/4

Tree is growing exponentially. Work at level $n^i = 4^i(cn/2^i) = 2^icn$
$1 = n/2^i -> 2^i = n -> i = log2(n)$
Therefore, work is $2^{log2(n)}cn = ncn = cn^2 = O(n^2)$

## Karatsuba
Observed that $(b-a)(c-d) + ac + bd = ad + bc$.
This method to calculate $ad + bc$ reduces the amount of multiplications by 1:
$ac, ad, bc, bd$ VS. $ac, bd, (b-a)(c-d)$

#### Psuedocode
```
Karatsuba(u,v,n):
	If (n = 1):
		Return u * v

	Let m <- [n/2]

	Write u = 10^m * a + b, v = 10^m * c + d

	Let e <- Karatsuba(a,c,m) // a*c
	Let f <- Karatsuba(b,d,m) // b*d
	Let g <- Karatsuba(b-a,c-d,m) // a*d + b*c

	Return 10^2m * e + 10m * (e + f + g) + f
```

#### Proof of Correctness
Show that Karatsuba can correctly multiply any digits.

**Base Case**:
$n = 1$, --> return $u*v$
Therefore, base case correctly multiplies $u*v$

**Inductive Hypothesis**:
Karatsuba is correct for All i-digit numbers, where ($1 \leq i \leq n$)

NTS: Karatsuba is correct for n-digit numbers
By the inductive hypothesis $a, b, c, d, b-a, c-d$ all have < n digits.
Therefore $e=ac$, $f = bd$, $g=(b-a)(c-d)$

#### Running Time
T(n) = runtime of Karatsuba on n-digit numbers
$T(n) = 3T(n/2) + n$: 3 recursive calls of $n/2$ amounts and $n$ additions.

								n
						      n/2 n/2 n/2
				n/4 n/4 n/4 n/4 n/4 n/4 n/4 n/4 n/4

Work at level $n^i = 3^i(n/2^i)$.
Last level: $log2(n)$ - See [here](#Finding-the-Last-Level)
Total work: $n * Sum(i=0, log_2 n, (3/2)^i)$

#### Geometric Series
Series $S = Sum(i=0, l-1, r^i)$
When r > 1, $S = O(r^l)$
When r < 1 $S = O(1)$

So for Karatsuba, $S = nc(3/2)^{log_2 n}$
$S = nc(3^{log_2 n}/2^{log_2 n})$
$S = nc(3^{log_2 n}/n)$
$S = c(n^{log_2 3})$
$S = O(n^{1.59})$

This is better than the original $O(n^1.59)$

# Finding the Last Level
$1 = size$ and then solve for $i$, where the last level is $i$

## Example
$1 = n/2^i$
$2^i = n$
$i = \lg n$

Therefore, the last level is $i$
