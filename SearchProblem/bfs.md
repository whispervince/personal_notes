### BFS

Use a a first-in, first-out (FIFO) queue as the fringe. Visit nodes in their order of insertion.

##### Properties

- BFS is complete. If a solution exists, then $$s$$ must be finite, BFS will eventually search the depth.

- BFS is not optimal in general because it does not consider the cost of each action. However when the cost of all actions are the same, BFS will find the optimal solution.

- The time complexity is $$O(b^s)$$. The number of nodes in the fringe is $$1 + b + b^2 + \ldots + b^s$$, which is $$O(b^s)$$.

- The space complexity is $$O(b^s)$$, which is the same as the time complexity.
