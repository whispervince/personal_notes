
### The Value of Perfect Information

In practice, it is difficult to make sure that the agent has all the information it needs for a particular problem. One of the most important parts of decision making is knowing whether or not it is worth gathering more evidence to help decide which action to take. The value of perfect information quantifies the amount an agent's MEU expected to increase if it observes some new evidence. Then we can compare the VPI of learning some new information with the cost to determine whether it is worthwhile to observe.

The MEU given current evidence $$e$$ is

$$MEU(e) = \max\limits_{a}\sum_{s}P(s|e)U(s,a)$$

where $$s$$ is any assignment to the chance nodes.

If we know that we observed some new evidence $$e'$$, the MEU will become

$$MEU(e,e') = \max\limits_{a}\sum_{s}P(s|e,e')U(s,a)$$

Because we do not know what new evidence we will get, we represent the new evidence as a random variable $$ E'$$ and compute the expected value of MEU

$$MEU(e, E') = \sum_{e'}P(e'|e)MSU(e,e')$$

Therefore the value of observing new evidence $$E'$$ given the current evidence $$e$$ is

$$VPI(E'|e) = MEU(e, E') - MEU(e)$$

##### Properties of VPI

- Nonnegativity - $$\forall E', e, VPI(E'|e) \ge 0$$

Observing new information always helps make a more informed decision, and the MEU can only increase or stay the same if the information is irrevalent

- Nonadditivity - $$VPI(E_j, E_k|e) \neq VPI(E_j|e) + VPI(E_k|e)$$ in general

Generally observing some new evidence $$E_j$$ might change how much we care about $$E_k$$.

- Order-Independence - $$VPI(E_j, E_k|e) = VPI(E_j|e) + VPI(E_k|e, E_j) = VPI(E_k|e) + VPI(E_j|e, E_k)$$

By intuition, it does not matter whether we observe the new evidences together or in some arbitrary sequential order. Observing multiple new evidences yields the same gain in MEU.