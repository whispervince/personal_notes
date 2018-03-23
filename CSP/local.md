### Local Search

Starts with some random assignment to values, then repeatedly selects the variable that violates the most constraints and resets it to the value that violates the fewest constraints (a policy known as the min-conflicts heuristic).

Appears to run in almost constant time and have a high probability of success.

Both incomplete and suboptimal, will not necessarily converge to an optimal solution.

Around $$R = \frac{\text{number of constraints}}{\text{number of variable}}$$, local search becomes extremely expensive.