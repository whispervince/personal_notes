### Model-Free Learning

##### Passive Reinforcement Learning

An agent is given a policy to follow and learns the value of states under that policy, which is exactly what is done by policy evaluation for MDPs when $$T$$ and $$R$$ are known.

##### Active Reinforcement Learning

An agent can use the feedback it receives to iteratively update the policy.

##### Direct Evaluation

Passive. Fix some policy $$\pi$$ and let the agent learn experience several episodes following $$\pi$$. Maintains counts of the total utility obtained from each state and the number of times it visited each state. At any time, the estimated value of any state $$s$$ can be calculated by dividing the total utility obtained from $$s$$ by the number of times $$s$$ was visited.

Direct evaluation is often unnecessarily slow to converge because it wastes information about transition between states.

##### Temporal Difference

Passive. Learns at every timestep. Uses the information about state transitions. Gives exponentially less weight to older, potentially less accurate samples. Converges to learning true state values much faster with fewer episodes than direct evaluation.

Uses exponential moving average to compute the weighted average represented by the Bellman equation.

**Sample value**: $$\text{sample} = R(s, \pi(s),s') + \gamma V^{\pi}(s')$$

**Update rule**: $$V^{\pi}(s) \leftarrow (1-\alpha)V^{\pi}(s) + \alpha \cdot \text{sample}$$

$$0 \le \alpha \le 1$$ is a parameter known as learning rate, which specifies the weights for the existing model and the new sampled estimate. It's typical to start out with $$\alpha = 1$$ and slowly shrink it to 0.

##### Q-Learning

Active. Computes the q-value of states directly, bypassing the need to know any values, transition functions, or reward functions, which are used to update the policy followed by the agent in passive learning.

**q-value iteration**: $$Q_{k+1}(s,a) = \sum_{s'}T(s, a, s')[R(s, a, s') + \gamma \max\limits_{a'}Q_k(s', a')]$$

**q-value sample**: $$\text{sample} = R(s, \pi(s),s') + \gamma \max\limits_{a'}Q(s', a')$$

**Update rule**: $$Q(s, a) \leftarrow (1-\alpha)Q(s, a) + \alpha \cdot \text{sample}$$

While TD learning and direct evaluation have to learn the values of states before determining policy optimality, Q-learning can learn the optimal policy directly by taking suboptimal or random actions, which is called off-policy learning.

##### Approximate Q-Learning

Sometimes we cannot visit all states during training and cannot store all q-values. Approximate q-learning tries to account for this by learning a few general situations and extrapolating to many similar situations. The key idea is the feature-based representation of states, which represents each state as a feature vector of dimension $$n$$, $$f(s)$$ or $$f(s,a)$$. Then the values of states can be written as

$$V(s) = w_1f_1(s) + w_2f_2(s) + \ldots + w_nf_n(s) = \vec{w}\cdot\vec{f}(s)$$

$$Q(s,a) = w_1f_1(s, a) + w_2f_2(s, a) + \ldots + w_nf_n(s, a) = \vec{w}\cdot\vec{f}(s, a)$$

**Difference**: $$\text{difference} = [R(s, a, s') + \gamma \max\limits_{a'}Q(s', a')] - Q(s,a)$$

**Update rule**: $$w_i \leftarrow w_i + \alpha \cdot \text{difference}\cdot f_i(s,a)$$

\* **Update rule for exact q-learning**: $$Q(s,a) \leftarrow Q(s,a) + \alpha \cdot \text{difference}$$

We only need to store a single weight vector and can compute q-values as needed.




