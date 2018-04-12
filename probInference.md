# Probabilistic Inference

### Introduction

##### Inference by Enumeration

Given a joint PDF, we can compute any desired probability $$P(Q_1, \ldots, Q_k | e_1, \ldots, e_k)$$ by collecting all the rows consistent with the evidence available ($$e_1, \ldots, e_k$$), and normalize the table to get a probability distribution.



### Bayes Nets - Representation

Takes advantage of the idea of conditional probability. Captures the relationships between variables by a large number of small local probability tables along with a directed acyclic graph. Each node represents a single random variable, arrows represent conditional probability distributions. 

A node is conditionally independent of all its ancestors given all of its parents. Thus at node $$Q$$, we store $$P(Q|X_1, \ldots, X_k)$$, where $$X_1, \ldots, X_k$$ are parents of $$Q$$. The conditional probability table at $$Q$$ has $$k+1$$ columns.







