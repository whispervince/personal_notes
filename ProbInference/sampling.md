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