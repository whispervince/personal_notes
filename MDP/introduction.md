### Introduction

A Markov Decision Process is defined by several properties

- A set of state $$S$$
- A set of actions $$A$$
- A start state
- Possibly one or more terminal states
- Possibly a discount factor $$\gamma$$
- A transition fn $$T(s,a,s')$$
- A reward fn $$R(s,a,s')$$

MDP can be modeled as search trees. Uncertainty is modeled with q-states, a.k.a. action states, which is identical to chance nodes.

##### Finite Horizons

Defines a lifetime for agents.

##### Discounting

We attempt to maximize discounted utility

$$U([s_0, a_0, s_1, a_1, s_2, \ldots]) = R(s_0, a_0, s_1) + \gamma R(s_1, a_1, s_2) + \gamma^2R(s_2,a_2,a_3) + \ldots$$

When $$|\gamma| < 1$$, the sum is guaranteed to be finite.

$$U([s_0, s_1, s_2, \ldots]) = \sum_{t = 0}^{\infty}\gamma^{t}R(s_t,a_t,s_{t+1}) \le \sum_{t=0}^{\infty}\gamma^{t}R_{\text{max}} = \frac{R_{\text{max}}}{1-\gamma}$$

where $$R_{\text{max}}$$ is the maximum possible reward attainable at any given timestep.

Typically, $$0 < \gamma < 1$$.

##### Markovianess

Markov property, a.k.a. memoryless property. 

$$P(S_{t+1} = s_{t+1}|S_t = s_t, A_t = a_t, S_{t-1} = s_{t-1}, A_{t-1} = a_{t-1}, \ldots, S_0=s_0)$$

equals  

$$P(S_{t+1} = s_{t+1}|S_t=s_t, A_t = a_t)$$

Encoded by the transition fn

$$T(s,a,s') = P(s'|s,a)$$



