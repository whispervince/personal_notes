### DFS

Use a last-in, first-out (LIFO) stack as the fringe. The most recently added object has the highest priority. 

##### Properties

- DFS is not complete. DFS will stuck if there exists any cycle in the graph.

- DFS is not optimal. It always returns the leftmost solution in the search tree.

- The time complexity is $$O(b^m)$$. In the worst case, DFS explores the entire tree before reach the goal state.

- The space complexity is $$O(bm)$$. At each level, there are at most $$b$$ nodes in the fringe.












