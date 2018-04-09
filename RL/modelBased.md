### Model-Based Learning

The agent keeps counts of the number of times it arrives in each state $$s'$$ after entering each q-state $$(s, a)$$. The agent can then generate the approximate transition function $$\hat{T}$$ by normalizing the counts it has collected and the reward function directly from the rewards it gets during exploration.

By the law of large numbers, as the agent collect more and more samples, the models of $$\hat{T}$$ and $$\hat{R}$$ will improve, with $$\hat{T}$$ converging to $$T$$ and $$\hat{R}$$ acquiring the knowledge of previously undiscoverd rewards. After certain period of exploration, we can generate a policy $$\pi_{\text{exploit}}$$ by running value or policy iteration with the current models for reward maximization. 

A disadvantage of model-based learning is that it can be expensive to maintain counts for every $$(s, a, s')$$ tuple.