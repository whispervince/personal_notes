### Solving MDP

Solving a MDP means finding an optimal policy $$\pi^*: S \rightarrow A$$, a fn mapping each state to an action. 

A policy $$\pi$$ defines a reflex agent which takes actions without considering future consequences. 

An optimal policy $$\pi^*$$ is one that if followed by the implementing agent, will yield the maximum expected total reward or utility. 

##### The Bellman Equation

Two mathematical quantities:

- The optimal value of a state $$s$$, $$V^*(s)$$ - the optimal value of $$s$$ is the expected value of the utility an optimally-behaving agent that starts in $$s$$ will receive over the rest of its lifetime.
- The optimal value of a q-state $$(s,a)$$, $$Q^*(s,a)$$ - the optimal value of $$(s,a)$$ is the expected value of the utility an agent receives after starting in $$s$$, taking $$a$$, and acting optimally henceforth.

The optimal value of a q-state is defined as:

$$Q^*(s,a) = \sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V^*(s')]$$

The Bellman equation is defined as:

$$V^*(s) = \max\limits_{a} \sum_{s'}T(s,a,s')[R(s,a,s') + \gamma V^*(s')]$$

or

$$V^*(s) = \max\limits_{a}Q^*(s,a)$$

The Bellman equation is an example of dynamic programming equation. The interpretation is that the optimal value of a state, $$V^*(s)$$, is the maximum expected utility over all possible actions from $$s$$.

The Bellman equation can be used as a condition for optimally. If a set of values $$V(s)$$ for every state $$s \in S$$ satisfies the Bellman equation for each of the states, we can conclude that these values are the optimal values, which means $$\forall s \in S, V(s) = V^*(s)$$.



