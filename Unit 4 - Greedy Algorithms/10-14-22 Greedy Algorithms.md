# Greedy Algorithm
Builds a solution by doing what is currently best, and does not look back
- Typically makes a single pass over input
- Uses a set of **Greedy Rules**.

Greedy algorithms are the fastest and simplest, and sometimes optimal.
- Simplicity makes them easy to adapt

# Unweighted Interval Scheduling
**INPUT**: n intervals
**OUTPUT** compatible schedule with largest possible size

## Possible Rules
- Keep adding shortest interval
	- Doesn't compute optimal solution (two long intervals overlapped by short interval)
- Add earliest interval first
	- Again not optimal (very long interval that starts early)
- Add earliest end interval first
	- Optimal (:

## Strategy
1. Sort intervals by finish time
2. Iterate over sorted list
3. If interval $i$ doesn't cause a conflict, add $i$ to $S$

Sort takes $O(nlogn)$
Iteration takes $O(n)$

Runtime is $O(nlogn)$

## Proof of Correction
**Greedy Stays Ahead** strategy to prove optimal rule.
- At every point in time, the greedy schedule does better than any other schedule.

Let $G$ be {$i_1$, ..., $i_r$} (greedy)
Let $O$ be {$j_1$, ..., $j_s$} (other)

*Key Claim*: For every interval index $t$ (1, ..., r): $f_i \leq f_j$ where $f$ represents finish time.

### Induction
$H(k)$: $f_i \leq f_j$ for all $1 \leq k \leq r$

*Base Case*: $H(1) = f_{i1} \leq f_{j1}$
- True, because we are choosing the earliest finish time for the first interval

*Induction*: $H(k-1) \longrightarrow H(k)$
*Inductive Hypothesis*: $f_{i k-1} \leq f_{j k-1}$ 

We want to prove that $f_{ik} \leq f_{jk}$.
Proof by contradiction (assume false): $f_{ik} > f_{jk}$
- We know that greedy can pick $f_{jk}$
- We know that greedy would pick $jk$ if it finished before $ik$ 
- Therefore, we have determined that our assumption is false ($f_{ik} > f_{jk}$)

Therefore, by our base case and inductive step, greedy is optimal.

# Minimum Lateness Scheduling
**INPUT:** n jobs with length $t_i$ and deadline $d_i$.
- All deadlines are distinct
**OUTPUT**: minimum-lateness schedule for jobs.
- Only do one job at a time
- The lateness of job $i$ is $max(f_i - d_i, 0)$
- Lateness of a schedule is $max(latenesses)$
**EXAMPLE**:
- Job 1 - Length (1) and Deadline (2)
- Job 2 - Length (2) and Deadline (4)
- Job 3 - Length (3) and Deadline (6)
Job 1 *finishes* at 1 (0 late), Job 2 *finishes* at 3 (0 late), and Job 3 *finishes* at 6 (0 late)
So the total lateness is 0.

## Possible Rules
### Shortest job
We will always choose the job with the shortest length. This fails when we have many short jobs with far deadlines, and many long jobs with close deadlines. These long jobs would be very late.

### Most urgent job
Urgency is defined as: ($d_i - t_i$). You would pick the job with the lowest urgency. This does not work when we have a short job with a further deadline than a longer task. For example Job 1 has length 1 and deadline 3, and Job 2 has length 5 and deadline 6. In this scenario, Job 2 is more urgent than Job 1. Therefore, when you do Job 2 and then Job 1, we get a lateness of 3. But if we do Job 1 and then Job 2, we get a lateness of 0. Therefore, we do not get the correct answer.

### Earliest deadline
We will always choose the job with the earliest deadline. Jobs will be ordered by increasing deadline and that will be our solution. We will then calculate the lateness of any job and find our solution.

## Proof of Correction
We will argue the correctness of our Greedy algorithm using an exchange argument.

## Exchange Argument
**Exchange Argument** is a strategy for proving greedy algorithms. The exchange argument tries to prove that G is the best schedule by swapping pairs of jobs.
Let *G* be our greedy schedule.
Let *O* be any other schedule.

### Rules
- We can transform *O* to *G* by exchanging pairs of jobs.
- Each exchange only *reduces* the lateness of *O*.
- Therefore, the lateness of *G* is at most that of *O*.

### Observations
- The optimal schedule has no gaps (all jobs should be schedules back-to-back)
- Two jobs, $i$ and $j$, are *inverted* in *O* if $d_i < d_j$ but $j$ is scheduled before $i$.
	- This is the *inverse* of our greedy rule for a pair of jobs.
	- The idea of *inversion* can be modified to fit our problems.

### Claim
An optimal schedule has *no inversions*.

Step 1: suppose *O* has an inversion, then it has an inverse $i$ and $j$ (where $i$ and $j$ are consecutive)
- Each job between two inverse jobs will have a deadline after the first job.
- Because of that, eventually the second job will have deadline earlier than the first job.
- Therefore, each non-inverted job between job 1 and job 2 will be a chain of inversions with job 2.
- A, C, D, E, F, B ; where B's deadline is before A. Therefore non-inverted jobs C, D, E and F's deadline must come after A. Therefore F is inverse B, as well as A, C, D and E.

Step 2: if $i$ and $j$ are *consecutive* that are *inverted*, then flipping them **reduces** the lateness.
- Case 1 - $d_j$ is not late in the inverted case. Therefore each job has a lateness of 0. When exchanged, they will still have lateness of 0. The lateness is no worse.
- Case 2 - $d_j$ is late in the inverted case. Therefore $j$ has a lateness of $s + t_i + t_j - d_j$. When exchanged, $j$ has a lateness of $s + t_j - d_j$ and $i$ has a lateness of $s + t_i + t_j - d_i$. If $t_i < 0$, then inverting $i$ and $j$ have reduced the lateness. Therefore, $d_i > d_j$