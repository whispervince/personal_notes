# Constraint Satisfaction Problems

### Introduction

CSPs are a type of identification problem, where we simply identify whether a state is a goal state or not, with no regard to how we arrive at that goal.

CSPs are defined by:

- Variables - CSPs possess a set of $$N$$ variables that can each take on a single value from some defined set of values
- Domian -  a set representing all possible values that a CSP variable can take on
- Constraints - restrictions on the values of variables

CSPs are NP-hard. Given a problem with $$N$$ variables with domain of size $$O(d)$$  for
each variable, there are $$O(d^N)$$ possible assignments. 

CSPs can be formulated as search problems:

- States - partial assignment
- Successor fn - outputs all states with one new variable assigned
- Goal test - verifies all variables are assigned and all constraints are satisfied



### Constraint Graph

Nodes represent variables and edges represent constraints.

Different types of constraints:

- Unary costraints - involves a single variable in the CSP, prunes the domain of the variables
- Binary constrains - involves two variables, represented in constraint graphs are traditional graph edges 
- Higher-order constraints - involves three or more variables, represented with edges in a graph, look unconventional



### Solving CSPs

Backtracking search, an optimization on DFS used specifically for CSPs.

**Improvements**

- Fixes an ordering for variables, and selects values for variables in this order. Valid because assignments are commutative.
- Only selects values that do not conflict with any previously assigned values. Backtracks and returns to the previous variable if no such values exist.

**Pseudocode**

```
function BACKTRACKING-SEARCH(csp) return a solution, or failure
	return RECURSIVE-BACKTRACKING({}, csp)

function RECURSIVE-BACKTRACKING(assignment, csp) return a solution, or failure
	if assignment is complete then return assignment
	var <- SELECT-UNASSIGNED-VARIABLE(VARIABLES[csp], assignment, csp)
	for each value in ORDER-DOMAIN-VALUES(var, assignment, csp) do
		if value is consistent with assignment given CONSTRAINTS[csp] then 
			add {val = value} to assignment
			result <- RECURSIVE-BACKTRACKING(assignment, csp)
			if result != failure then return result
			remove {val = value} from assignemnt
	return failure
```



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

Let $$e$$  be the number of arcs and $$d$$ be the size of the largest domain, the AC-3 algorithm has a worst case time complexity of $$O(ed^3)$$. Arc consistency requires more computation to enforce but leads to fewer backtracks. 

##### K-consistency

Guarantees that for any set of $$k$$ nodes in the CSP, a consistent assignment to and subset of $$k-1$$ nodes guarantees that the $$k^\text{th}$$ node has at least one consistent value.

Strong k-consistency guarantees that any subset of $$k$$ nodes is not only $$k$$-consistent but also $$k-1, k-2, \ldots, 1$$ consistent as well.

A higher degree of consistency on a CSP is more expensive to compute.

Increaing degrees of consistency:

- $$1$$-consistency - node consistency, unary constraints
- $$2$$-consistency - arc consistency
- $$3$$-consistency - path consistency
- $$n$$-consistency - solves CSP without backtracking



### Ordering

Gives ordering for both the variables and values involved.

##### Minimum Remaining Values

Chooses whichever unassigned variable has the fewest valid remaining values (the most constrainted variable). By intuition, the most constraint variable is most likely to run out of possible values and result in backtracking, and so it is best to assign a value to it sooner than later.

##### Least Constraining Value

When selecting which value to assign next, chooses the value that prunes the fewest values from the domains of the remaining unassigned values. Requires additional computation (rerunning filtering) but still yields speed gains depending on usage.



### Structure

Consider a tree-structured CSP (no loops in constraint graph), we can reduce the runtime for finding a solution from $$O(d^N)$$ to $$O(Nd^2)$$.

**Idea**:

- Pick a random node in the constraint graph as the root of the tree.
- Convert all undirected edges in the tree to directed edges that point away from the root. Then linearize (or topologically sort) the directed acyclic graph.
- Perform a backwards pass of arc consistency, enforce arc consistency for all arcs $$\text{Parent}(i) \longrightarrow i$$
- Perform a forward assignment. Because we have enforced arc consistency on all of the arcs, the children will each have at least one consistent value (proved by induction). The iterative assignment guarantees a correct solution.

##### Cutset Conditioning

First find the smallest subset of variables such that their removal results in a tree (a cutset for the graph). Assign all variables in it and prune the domains of all neighboring nodes. Then solve the resulting tree-structured CSP(s). 

Let $$c$$ be the size of the cutset, the runtime will be $$O(d^c(n-c)d^2)$$, where $$c$$ is small.






