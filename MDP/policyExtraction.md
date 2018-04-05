### Policy Extraction

Used to determine an optimal policy after all optimal values for states are calculated. 

$$\forall s \in S, \pi^*(s) =\underset{x}{\mathrm{argmax}}\  Q^*(s,a) = \underset{x}{\mathrm{argmax}}\  \sum_{s'}T(s,a,s')[R(s,a,s') + \gamma V^*(s')]$$

If we have the optimal q-values of states, an argmax operation is enough to determine the optimal action from a state. If we store only $$V^*(s)$$, we have to recompute all necessary q-values, which is equivalent to performing a depth-1 expectimax.



