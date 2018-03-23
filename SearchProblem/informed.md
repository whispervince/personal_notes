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