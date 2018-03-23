### UCS - Uniform Cost Search

Use a priority queue as the fringe, where the weight for a node $$v$$ is the path cost from the start node to $$v$$. Each time remove a node with the lowest path cost from the fringe.

##### Properties

- UCS is complete. If a solution exists, then the shortest path cost must be finite, UCS will eventually find the path.
- UCS is optimal if all edge costs are non-negative, which guaruntees that the nodes are explored in order of increasing path cost. The idea is the same as Dijkstra's algorithm.
- The time complexity if $$O(b^\frac{C^*}{\epsilon})$$. Let $$C^*$$ be the optimal cost, $$\epsilon$$ be the minimum cost between two nodes, we will explore $$O(b^\frac{C^*}{\epsilon})$$ nodes. 
- The space complexity is $$O(b^\frac{C^*}{\epsilon})$$, which is the same as the time complexity.