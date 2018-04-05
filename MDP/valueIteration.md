### Value Iteration

The time-limited values for a state $$s$$ with a time-limit of $$k$$ timesteps, $$V_k(s)$$, represents the maximum expected utility attainable from $$s$$ given that the MDP terminates in $$k$$ timesteps.

Value iteration is a DP algorithm that computes time-limited values until convergence $$\forall s, V_{k+1}(s) = V_k(s)$$

##### Idea

1. Initialize $$V_0(s) = 0, \forall s \in S$$. A time limit of 0 timesteps means no actions can be taken, which means that no rewards can be acquired.

2. Repeat the update rule until convergence:

   $$\forall s \in S, V_{k+1}(s) = \max\limits_a \sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V_k(s')]$$

In each iteration, we must update the values of all $$|S|$$ states. At each state, we need iterate over $$|A|$$ actions. The computation of these q-values requires iteration over each of the $$|S|$$ states again. Therefore the runtime will be $$O(|S|^2|A|)$$. (All possible $$(s, a, s')$$ tuples)



