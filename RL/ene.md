### Exploration and Exploitation

##### $$\mathcal{E}$$-Greedy Policies

Defines some probability $$0 \le \epsilon \le 1$$. Acts randomly and explores with probability $$\epsilon$$, and follows the current established policy and exploits with probability $$1-\epsilon$$.

$$\epsilon$$ has to be manually tuned and lowered over time.

##### Exploration Functions

**Modified q-value sample**: $$\text{sample} = R(s, \pi(s),s') + \gamma \max\limits_{a'}f(s', a')$$

$$f$$ denotes an exploration function, and a common choice is $$f(s,a) = Q(s,a) + \frac{k}{N(s,a)}$$, where $$k$$ is a predetermined value and $$N(s,a)$$ is the number of times q-state $$(s,a)$$ has been visited.

Exploration is encoded by $$f$$ since the term $$\frac{k}{N(s,a)}$$ gives a bonus to some infrequently-taken actions. As time goes on, the bonus decreases to 0 and $$f(s,a)$$ regresses to $$Q(s,a)$$, making exploitation more and more exclusive.
