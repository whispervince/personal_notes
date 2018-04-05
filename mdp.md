# Markov Decision Processes






### Policy Extraction

Used to determine an optimal policy after all optimal values for states are calculated. 

$$\forall s \in S, \pi^*(s) =\underset{x}{\mathrm{argmax}}\  Q^*(s,a) = \underset{x}{\mathrm{argmax}}\  \sum_{s'}T(s,a,s')[R(s,a,s') + \gamma V^*(s')]$$

If we have the optimal q-values of states, an argmax operation is enough to determine the optimal action from a state. If we store only $$V^*(s)$$, we have to recompute all necessary q-values, which is equivalent to performing a depth-1 expectimax.



### Policy Iteration

##### Idea

1. Define an initial policy.

2. Repeat the following until convergence:

   1. Evaluate the current policy. For a policy $$\pi$$, compute $$V^{\pi}(s)= \sum_{s'}T(s,\pi(s),s')[R(s,\pi(s),s')+\gamma V^{\pi}(s')]$$ 

      Define the policy at iteration $$i$$ is $$\pi_{i}$$. Since we are fixing a single action for each state, each $$V^{\pi_i}(s)$$ can be computed by solving a linear system. 

      Alternatively, $$V^{\pi_i}(s)$$ can be computed by using the following update rule until convergence

      $$V^{\pi_i}_{k+1}(s) = \sum_{s'}T(s,\pi(s), s')[R(s,\pi(s),s') + \gamma V_{k}^{\pi_i}(s')]$$

      The second method is typically slower in practice.

   2. Use policy improvement to generate a better policy

      $$\pi_{i+1}(s) = \underset{a}{\mathrm{argmax}} \sum_{s'}T(s, a, s')[R(s, a, s')+\gamma V^{\pi_i}(s')]$$

      If $$\pi_{i+1} = \pi_i$$, the algorithm has converged, and we can conclude $$\pi^* = \pi_{i+1} = \pi_{i}$$

