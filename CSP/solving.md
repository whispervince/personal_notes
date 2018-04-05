### Backtracking Search

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


