# Games






### Expectimax

Use chance nodes, which compute the expected utility over children, instead of minimizers.

$$\forall \text{ chance states, } V(s) = \sum\limits_{s' \in \text{successor}(s)} p(s')V(s')$$

Note that the minimax is a special case of expectimax, where the lowest-value child $$s^*$$ of a minimizer has $$p(s^*) = 1$$

##### Pseudocode

Replace the minimizer with chance nodes.

```python
def exp-value(state):
	initialize v = 0
	for each successor of states:
		p = probability(successor)
		v += p * values(successor)
	return v
```



### General Games

Different agents have distinct tasks. The trees are characterized by multi-agent utilities, which are represented as tuples with different values corresponding to unique utilities for different agents. Each agent then attempts to maximize their own utility at each node they control, ignoring the utilities of other agents.