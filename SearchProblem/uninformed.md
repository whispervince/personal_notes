### Uninformed Search

Maintain an outer fringe of partial plans. Expand our fringe by removing a node and replacing
it on the fringe with all its children. Continue this until eventually removing a goal state off the fringe.

Known as tree search.

##### Pseudocode

```
function TREE-SEARCH(problem, fringe) return solution, or failure
fringe <- INSERT(MAKE-NODE(INITIAL-STATE[problem]), fringe)
loop do
if fringe is empty then return failure
node <- REMOVE-FRONT(fringe)
if GOAL-TEST(problem, STATE[node]) then return node
for child-node in EXPAND(STATE[node], problem) do
fringe <- INSERT(child-node, fringe)
end
end
```

##### Strategies

DFS, BFS, UCS (Uniform Cost Search). Different strategies have different fringe representation.

##### Properties

- Completeness: If there exists a solution to the search problem, whether the strategy is guaranteed to find a solution.

- Optimality: whether the strategy is guaranteed to find the lowest cost path to a
goal state.

- Branching factor $$b$$: The increase in the number of nodes on the fringe each time a fringe node
is dequeued and replaced with its children is $$O(b)$$. At depth $$k$$ in the search tree, there exists $$O(b^k)$$ nodes.

- Maximum depth $$m$$

- The depth of the shallowest solution $$s$$
