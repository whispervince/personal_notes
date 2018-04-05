# Markov Decision Processes


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



### Value Iteration

The time-limited values for a state $$s$$ with a time-limit of $$k$$ timesteps, $$V_k(s)$$, represents the maximum expected utility attainable from $$s$$ given that the MDP terminates in $$k$$ timesteps.

Value iteration is a DP algorithm that computes time-limited values until convergence $$\forall s, V_{k+1}(s) = V_k(s)$$

##### Idea

1. Initialize $$V_0(s) = 0, \forall s \in S$$. A time limit of 0 timesteps means no actions can be taken, which means that no rewards can be acquired.

2. Repeat the update rule until convergence:

   $$\forall s \in S, V_{k+1}(s) = \max\limits_a \sum_{s'}T(s,a,s')[R(s,a,s')+\gamma V_k(s')]$$

In each iteration, we must update the values of all $$|S|$$ states. At each state, we need iterate over $$|A|$$ actions. The computation of these q-values requires iteration over each of the $$|S|$$ states again. Therefore the runtime will be $$O(|S|^2|A|)$$. (All possible $$(s, a, s')$$ tuples)



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

