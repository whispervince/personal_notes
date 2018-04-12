# Probabilistic Inference

### Introduction

##### Inference by Enumeration

Given a joint PDF, we can compute any desired probability $$P(Q_1, \ldots, Q_k | e_1, \ldots, e_k)$$ by collecting all the rows consistent with the evidence available ($$e_1, \ldots, e_k$$), and normalize the table to get a probability distribution.



### Bayes Nets - Representation

Takes advantage of the idea of conditional probability. Captures the relationships between variables by a large number of small local probability tables along with a directed acyclic graph. Each node represents a single random variable, arrows represent conditional probability distributions. 

A node is conditionally independent of all its ancestors given all of its parents. Thus at node $$Q$$, we store $$P(Q|X_1, \ldots, X_k)$$, where $$X_1, \ldots, X_k$$ are parents of $$Q$$. The conditional probability table at $$Q$$ has $$k+1$$ columns.



### Bayes Nets - Inference

Inference is the process of calculating the joint PDF for some set of query variables based on some set of observed variables.

##### Naive Method

Forms the joint PDF and uses inference by enumeration.

##### Variable Elimination

To eliminate a variable $$X$$:

1. Join all factors involving $$X$$
2. Sum out $$X$$

The order of elimination determines the maximum size of any factors generated, which is usually smaller than the entire joint PDF.



### Bayes Nets - Sampling

Another approach for probabilistic reasoning is to implicitly calculate the probabilities for the query by simply counting samples. 

##### Prior Sampling

Generate a large number of samples, collect all samples consistent with the evidence, and compute the distribution.

##### Rejections Sampling

Similar to prior sampling but early rejects any sample inconsistent with evidence.

##### Likelihood Weighting

Ensures that we never generate a bad sample. In order to make sure that the sample actually obeys the joint PDF, we use a weight for each sample, which is the probability of the evidence variables given the sampled variables. In each iteration, we sample a value if the variable is not an evidence variable, or update the weight if the variable is evidence.

For all three sampling methods above, we can increase accuracy by generating additional samples. However, likelihood weighting is the most computationally efficient one among the three methods.

##### Gibbs Sampling

First set all variables to totally random values. Repeatedly pick one variable and resample it given the currently assigned values of all other variables. In a typical Bayes net, where most variables have only a few neighbors, we can precompute the conditional distributions for each variable given all of its neighbors in linear time. If we repeat the process enough times, the later samples will converge to the correct distribution.


