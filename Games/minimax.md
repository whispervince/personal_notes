### Minimax

##### Utility

Defines the value of a state.

The value of a terminal state, called a terminal utility, is always some deterministic value. A state's value is defined as the best possible outcome (utility) an agent can achieve from that state. 

$$\forall \text{ agent-controlled states, } V(s) = \max\limits_{s' \in \text{ successors}(s)} V(s')$$

$$\forall \text{ opponent-controlled states, } V(s) = \min\limits_{s' \in \text{ successors}(s)} V(s')$$

$$\forall \text{ terminal states, } V(s) \text{ is known}$$

##### Postorder Traversal Pseudocode

```python
def value(state):
	if the state is a terminal state: return the utility of the state
	if the next agent is MAX: return max-value(state)
	if the next agent is MIN: return min-value(state)

def max-value(state):
	initialize v = -inf
	for each successor of state:
		v = max(v, value(successor))
	return v

def min-value(state):
	initialize v = inf
	for each successor of state:
		v = min(v, value(successor))
	return v
```

##### Alpha-Beta Pruning

Let $$\alpha$$ be the maximum a maximizer can get, $$\beta$$ be the minimum a minimizer can get. At a minimizer, if any successor's value is less than the current $$\alpha$$, the maximizer above will not choose the value from this node, and therefore we do not have to look at the rest successors. At a maximizer, if any successor's value is greater then the current $$\beta$$, the minimizer above will not choose the value from this node, and therefore we do not have to look at the other successors.

##### Pseudocode with Pruning

```python
def max-value(state, alpha, beta):
	initialize v = -inf
	for each successor of state:
		v = max(v, value(sccessor, alpha, beta))
		if v >= beta return v
		alpha = max(alpha, v)
	return v

def min-value(state, alpha, beta):
	initialize v = inf
	for each successor of state:
		v = min(v, value(successor, alpha, beta))
		if v <= alpha return v
		beta = min(beta, v)
	return v
```

##### Evaluation Functions

Functions that take in a state and output an estimate of the true minimax value of the state. Widely exployed in depth-limited minimax. Serves a similar purpose in games as heuristics in standard search problems. Common design for an evaluation function is a linear combination of features.



