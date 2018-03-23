### Ordering

Gives ordering for both the variables and values involved.

##### Minimum Remaining Values

Chooses whichever unassigned variable has the fewest valid remaining values (the most constrainted variable). By intuition, the most constraint variable is most likely to run out of possible values and result in backtracking, and so it is best to assign a value to it sooner than later.

##### Least Constraining Value

When selecting which value to assign next, chooses the value that prunes the fewest values from the domains of the remaining unassigned values. Requires additional computation (rerunning filtering) but still yields speed gains depending on usage.









