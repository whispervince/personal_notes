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



