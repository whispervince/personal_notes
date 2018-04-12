# Decision Networks

### Introduction

A combination of Bayes nets and expectimax. 

- Chance nodes - Each outcome in a chance node has an associated probability, which can be determined by running inference. Represented with ovals.
- Action nodes - Represents a choice between a number of actions that we have the power to choose from. Represented with rectangles.
- Utility nodes - Children of some combination of action and chance nodes. Outputs a utility based on the values taken on by the parents. Represented with diamonds.

Our goal with decision networks is to select the action which yields the maximum expected utility. This can be achieved with the following procedure:

1. Instantiate all known evidence and run inference to calculate the posterior probabilities of all chance node parents of the utility node into which the action node feeds 

2. Go through each possible action and compute the expected utility of taking that action given the posterior probabilities calculated in the previous step.

   The expected utility of taking action $$a$$ given evidence $$e$$ and $$n$$ chance nodes is 

   $$EU(a|e) = \sum_{x_1,\ldots,x_n}P(x_1,\ldots,x_n|e)U(a,x_1,\ldots,x_n)$$

   where each $$x_i$$ represents a value that the $$i$$-th chance node can take on.

3. Select the action which yields the highest expected utility to get the MEU.

   $$MEU(e) = \max\limits_{a}EU(a|e)$$

