# Search Problem

### Agent

**Reflex agent**: selects an actions solely based on the current state. 

**Planning agent**: maintains a model of the world, simulates performing various actions, determines hypothesized consequences of actions, and selects the best one

Planning agent usually outperforms reflex agents.



### Search Problem

A search problem is formulated by:

- State space - the set of all possible states in the given world
- Successor fn - a fn that takes in a state and an action, and computes the cost of the action and the successor states
- Start state - the initial state of the agent
- Goal test - a fn that takes in a state and determines that whether it is a goal state

A search problem is solved by starting from the start state and iteratively exploring the state space by calling the successor fn until reaching the goal state, at which point a path is determined.

##### State Space Graphs

Constructed with states representing nodes, with directed edges existing from a state to its successors. These edges represent actions, and any associated weights represent the cost of performing the corresponding action. In a graph, each state is represented exactly once.

##### Search Trees

Each node encodes  the entire path from the start state to the given state in the state space graph.

No restriction on the number of times a state can appear. Greater than or equal to their corresponding state space graph in size.



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



### DFS

Use a last-in, first-out (LIFO) stack as the fringe. The most recently added object has the highest priority. 

##### Properties

- DFS is not complete. DFS will stuck if there exists any cycle in the graph.

- DFS is not optimal. It always returns the leftmost solution in the search tree.

- The time complexity is $$O(b^m)$$. In the worst case, DFS explores the entire tree before reach the goal state.

- The space complexity if $$O(bm)$$. At each level, there are at most $$b$$ nodes in the fringe.



### BFS

Use a a first-in, first-out (FIFO) queue as the fringe. Visit nodes in their order of insertion.

##### Properties

- BFS is complete. If a solution exists, then $$s$$  must be finite, BFS will eventually search the depth.

- BFS is not optimal in general because it does not consider the cost of each action. However when the cost of all actions are the same, BFS will find the optimal solution.

- The time complexity is $$O(b^s)$$. The number of nodes in the fringe is $$1 + b + b^2 + \ldots + b^s$$, which is $$O(b^s)$$.

- The space complexity is $$O(b^s)$$, which is the same as the time complexity.



### UCS - Uniform Cost Search

Use a  priority queue as the fringe, where the weight for a node $$v$$ is the path cost from the start node to $$v$$. Each time remove a node with the lowest path cost from the fringe. 

##### Properties

- UCS is complete. If a solution exists, then the shortest path cost must be finite, UCS will eventually find the path.
- UCS is optimal if all edge costs are non-negative, which guaruntees that the nodes are explored in order of increasing path cost. The idea is the same as Dijkstra's algorithm. 
- The time complexity if $$O(b^\frac{C^*}{\epsilon})$$. Let $$C^*$$ be the optimal cost, $$\epsilon$$  be the minimum cost between two nodes, we will explore $$O(b^\frac{C^*}{\epsilon})$$ nodes. 
- The space complexity is $$O(b^\frac{C^*}{\epsilon})$$, which is the same as the time complexity.



### Informed Search

If we have some notion of the direction in which we should focus our search, we can improve the performance. 

##### Heuristics

An estimation of distance to goal states, typically the solution to relaxed problems, where some of the constraints are removed. 

##### Greedy Search

Use a  priority queue as the fringe, where the weight for a node $$v$$ is the heuristic value (estimated forward cost) of the node. Each time remove the fringe node with lowest heuristic value, which corresponds to the state it believes is nearest to a goal.

Greedy search is neither optimal nor complete. It depends on the goodness of the heuristic function.

##### A$$^*$$ Search

Use a  priority queue as the fringe, where the weight for a node $$v$$ is the estimated total cost (the sum of the total backward cost and the estimated forward cost) of the node. Each time remove the fringe node with lowest estimated total cost.

Given an appropriate heuristic, A$$^*$$ search is guarunteed to be both complete and optimal. It is a combination of the generally high speed of greedy search and the completeness and optimality of UCS.



### Admissibility and Consistency

Define

- $$g(n)$$ - total backwards cost (in UCS)
- $$h(n)$$ - estimated forward cost (in greedy)
- $$f(n)$$ - estimated total cost, $$g(n) + h(n)$$

##### Admissibility

Let $$h^*(n)$$ be the true optimal forward cost to reach a goal state from node $$n$$. Heuristic $$h(n)$$ is admissible if $$\forall n, 0 \le h(n) \le h^*(n)$$, and $$h(G) = 0$$ for any goal state $$G$$.

**Thm**: For a given search problem, if the admissibility constraint is satisfied by a heuristic function $$h$$, using A* tree search with $$h$$ on that search problem will yield an optimal solution.

Tree search could fail by searching the same cycle in the state space graph. It revisits the same node multiple times if there are multiple ways the get to the same node. We can get rid of this issue by keeping track of the states that are already expanded. 

##### Graph Search Pseudocode

```
function GRAPH-SEARCH(problem, fringe) return a solution, or failure
	closed <- an empty set
	fringe <- INSERT(MAKE-NODE(INITIAL-STATE[problem]),fringe)
	loop do
		if fringe is empty then return failure
		node <- REMOVE-FRONT(fringe)
		if GOAL-TEST(problem, STATE[node]) then return node
		if STATE[node] is not in closed then
			add STATE[node] to closed
			for child-node in EXPAND(STATE[node], problem) do
				fringe <- INSERT(child-node, fringe)
			end
	end
```

**Consistency**

Heuristic $$h(n)$$ is consistent if $$\forall A, C, h(A) - h(C) \le cost(A,C)$$, and $$h(G) = 0$$ for any goal state $$G$$.

**Thm**: For a given search problem, if the consistency constraint is satisfied by a heuristic function $$h$$, using A* graph search with $$h$$ on that search problem will yield an optimal solution.

Consistency implies adimissibility.

##### Dominance

Heuristic $$a$$ is dominant over heuristic $$b$$ if $$\forall n, h_a(n) \ge h_b(n)$$.

If one admissible/consistent heuristic is dominant over another, it must be better because it will always more closely estimate the distance to a goal from any given state.

The maximum among multiple admissible/consistent heuristics will also be admissible/consistent.



