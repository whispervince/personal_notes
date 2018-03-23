### Filtering

Checks if we can prune the domains of unassigned variables ahead of time by removing values we know will result in backtracking.

##### Forward Checking

Whenever a value is assigned to a variable $$i$$, prunes the domains of unassigned variables that share a constraint with $$i$$ that would violate the constraint if assigned.

##### Arc Consistency

Interprets each undirected edge of the constraint graph as two directed edges pointing in opposite directions. Each directed edge is called an arc.

**Idea**:

- Stores all arcs in a queue $$Q$$.
- Iteratively removes arcs from $$Q$$. For each removed arc $i \longrightarrow j$, for every remaining value $$v$$ for the tail variable $$i$$, there is at least one remaining value $$w$$ for the head variable $$j$$ such that $$i = v$$, $$j = w$$ does not violate any constraints. Removes $$v$$ from the domain of $$i$$ if no such $$w$$ exists. 
- If at least one value is removed for $$i$$, add arcs $k \longrightarrow i, \forall k$ unassigned. If the arc is already in $$Q$$, it does not need to be added again.
- Repeats until Q is empty, or the domain of some variable is empty and triggers a backtrack.

##### Arc Consistency Algorithm #3

**Pseudocode**

```
function AC-3(csp) return the CSP, possibly with reduced domains
queue <- all arcs in csp
while queue is not empty do
(i, j) <- REMOVE-FIRST(queue)
if REMOVE-INCONSISTENT-VALUES(i, j) then
for each k in NEIGHBORS[i] do
add (k, i) to queue

function REMOVE-INCONSISTENT-VALUES(i, j) return true iff succeed
removed <- false
for each x in DOMAIN[i] do
if no value y in DOMAIN[j] allows (x, y) to satisfy the constraint (i <-> j)
then delete x from DOMAIN[i], removed <- true
return removed
```

Let $$e$$ be the number of arcs and $$d$$ be the size of the largest domain, the AC-3 algorithm has a worst case time complexity of $$O(ed^3)$$. Arc consistency requires more computation to enforce but leads to fewer backtracks.

##### K-consistency

Guarantees that for any set of $$k$$ nodes in the CSP, a consistent assignment to and subset of $$k-1$$ nodes guarantees that the $$k^\text{th}$$ node has at least one consistent value.

Strong k-consistency guarantees that any subset of $$k$$ nodes is not only $$k$$-consistent but also $$k-1, k-2, \ldots, 1$$ consistent as well.

A higher degree of consistency on a CSP is more expensive to compute.

Increaing degrees of consistency:

- $$1$$-consistency - node consistency, unary constraints
- $$2$$-consistency - arc consistency
- $$3$$-consistency - path consistency
- $$n$$-consistency - solves CSP without backtracking
