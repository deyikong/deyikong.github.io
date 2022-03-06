---
title: Fundamental
date: 2022-03-05 19:51:47
tags:
---

###  Loop Invariants
- *Initialization*: It's true prior to the first iteration of the loop(variable initializations before the loop)
- *Maintenance*: If it's true before an iteration of the loop, it remains true before the next iteration(logic inside the loop) 
- *Termination*: When the loop terminates, the invariant gives us a useful property that helps show that the algorithm is correct(condition to stop the loop) 
   
Initialization -> base case, Maintenance -> inductive step, Termination -> Condition; Termination usually involves loop invariants. In math, inductive steps carries on infinitely, but in here, we stop the loop when the termination happens. 

**Notes:** practice always come up with the loop invariants before you start writing the loop. 

### Running time
Big O notation
### Worst-cast and average-case analysis
Worst-Case
- an upper bound on teh running time
- for some algorithms, the worst case occurs fairly often. Like searching a database for a piece of missing data.   

### Designing Algorithms
Principles: 
- Incremental: e.g.: insertion sort, insert single emlement into its proper place.
- Divide and conquer: 
  * *recurisve* is from this principle
  * three steps:
    + **Divide** into smaller subproblems
    + **Conquer** the subproblems by solving them recursively
    + **Combine** the solution