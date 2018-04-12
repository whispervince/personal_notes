### Bayes Nets - Inference

Inference is the process of calculating the joint PDF for some set of query variables based on some set of observed variables.

##### Naive Method

Forms the joint PDF and uses inference by enumeration.

##### Variable Elimination

To eliminate a variable $$X$$:

1. Join all factors involving $$X$$
2. Sum out $$X$$

The order of elimination determines the maximum size of any factors generated, which is usually smaller than the entire joint PDF.