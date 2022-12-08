# Huffman Codes
We are going to prove Huffman Codes optimality using exchange argument.

## Theorem
Huffman's Algorithm produces an optimal prefix-free code.
1. In an optimal prefix-free code, every node has exactly two children.
2. If $x, y$ have the lowest frequencies, then $x,y$ are the lowest level of the tree in any optimal code.

If either of the previous two rules are swapped, then we have an inverted solution.

## Exchange Argument
In the optimal solution, the two least frequent letters are siblings at the lowest level.
The remaining portion of the optimal tree is an optimal solution for the remaining problem.
- Letters 3 through $n$ (rest of nodes)
- A new letter with frequency $f_1 + f_2$ (Bottom two nodes)

## Induction
### Base Case
We have two elements left. Therefore, there is an obvious choice for what tree we should make.

### Inductive Hypothesis
$H(k-1) \longrightarrow H(k)$

### Inductive Step
- Frequencies are $f_1, ..., f_k$ and the two lowest are $f_1$ and $f_2$.
- Apply the Huffman Coding algorithm and combine to new letter $f_w$
- By the inductive hypothesis, $H(k-1)$ is true
	- Therefore we know $f_3, ..., f_k$ is already optimal
Therefore, we know that every solution is optimal by induction.

# Fractional Knapsack
Every item can be cut or divided into arbitrarily small quantities. Each item $i$ has a weight $w_i$ and value $v_i$.

We need to determine the fractions of items to select, with a maximum weight of $W$, so that the *total value* is maximized.

## Greedy Rule
Pick the item with the largest $v_i / w_i$ ratio. This represents the fractions we can divide the item into. Fit as many parts of that item into the knapsack as possible. Repeat until the knapsack is filled.

## Exchange Argument
Suppose $v_1 / w_1$ > $v_2 / w_2$ etc.
- Compare Greedy solution to another solution $O$
- Let $j$ be the first item where the two solutions differ
- The quotient of $j$ in $G$ is greater than in $O$
- In $O$, we can replace an item further right than $j$ without decreasing the total value
- $O$ matches $G$ in one more item.
Therefore we can swap to create an optimal solution.

# Exchange Argument
Show how any other solution (including optimal) can be transformed to the greedy solution
- Ensure that each transformation does not make the solution worse
- Transformations should not remove or add date, simply "swap"

# Greedy Stays Ahead
Prove that after each step, the greedy solution is at least as good as any other solution
- Can be done using induction