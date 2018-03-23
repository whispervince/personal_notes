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















