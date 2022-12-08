# Labor Markets
Most labor markets are frustrating
- Not everyone can get favorite job/candidate
- Leads to potential **chaos**

Decentralized labor markets are confusing
- You get an offer from your 2nd choice, what should you do?
- Accept - Could have been happier if #1 offers
- Decline - #1 never offers you are not happy you die alone

## Centralized Labor Markets
What if we could just assign jobs? No free will required.
- What information do we need?
	- Jobs available
	- Applicants/Companies
	- Preferences of applicants and companies
- What type of assignment would prevent the earlier chaos of people switching jobs.
	- Stable assignment (no employee/employer pair that is prefer each other to their assignment).

# Stable Matching Problem
We are going to do this on an example of Doctors and Hospitals.

## Input
- $n$ doctors
- $n$ hospitals
- Each doctors ranking of hospitals
- Each hospitals ranking of doctors

## Matchings
A **matching** $M$ is a set of doctor-hospital pairs where no doctor/hospital appears twice
- **Perfect matching** every doctor/hospital pair appears once

A *matching* $M$ can be **unstable** if some doctor/hospital pair prefer one another to their match in $M$
- Think of two doctors who are assigned to each others #1 hospital. They can swap and then the matching would be stable.

## Example
**Hospital Preferences**:
| --- | 1st   | 2nd   | 3rd   |
| --- | ----- | ----- | ----- |
| MGH | Alice | Bob   | Clara |
| BW  | Bob   | Clara | Alice |
| BID | Alice | Clara | Bob   | 

**Doctor Preferences**:
| ---   | 1st | 2nd | 3rd |
| ----- | --- | --- | --- |
| Alice | BW  | BID | MGH |
| Bob   | BID | MGH | BW  |
| Clara | MGH | BID | BW    |

**Stable Matching**
| Doctor | Hospital |
| ------ | -------- |
| Alice  | BW       |
| Bob    | BID      |
| Clara  | MGH      | 
This is a stable matching, because no doctor would rather be at a different hospital.

# Gale-Shapley Algorithm
National system for matching US medical school graduates to medical residencies
- Roughly 40,000 doctors per year
- Assignment is almost entirely algorithmic

```
Gale-Shapley():
	Let M = []
	While (some hospital h not matched):
		If (h has offered a job to everyone): break
		Let d = highest ranked doctor not yet offered a job
		h.offerJob(d)

offerJob(d):
	If (d is unmatched):
		d accepts; 
		M.add(d, h); return;
	If (d is matched to h', and d prefers h'):
		d rejects; return;
	If (d is matched to h', and d prefers h):
		M.remove(d, h')
		d accepts
		M.add(d, h)
```
## Observations
- Hospitals make offers in descending order (Hospital happiness decreases)
- Doctors accept offers in ascending order (Hospitals happiness increases)
- Doctors that have a job never become unemployed

## Questions
- Will this algorithm terminate? How long will it take?
- Does it output a perfect matching?
- How do we implement this algorithm efficiently

## Termination
**Claim**: The algorithm terminates after at most $n^2$ iterations of the main loop
- Only at most $\leq n^2$ offers
- All hospitals can only offer once to each doctor

## Efficiency
A straightforward implementation takes $O(n^3)$ time and $O(n^2)$ space.
- $O(n^2)$ time for each hospital offer
- $O(n)$ time to check if one hospital is better than another

Instead we can improve this by using a Map for each doctor, from hospital name to rank.
Then we can compare hospitals in $O(1)$ time.