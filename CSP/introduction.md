### Introduction

CSPs are a type of identification problem, where we simply identify whether a state is a goal state or not, with no regard to how we arrive at that goal.

CSPs are defined by:

- Variables - CSPs possess a set of $$N$$ variables that can each take on a single value from some defined set of values
- Domian -  a set representing all possible values that a CSP variable can take on
- Constraints - restrictions on the values of variables

CSPs are NP-hard. Given a problem with $$N$$ variables with domain of size $$O(d)$$  for
each variable, there are $$O(d^N)$$ possible assignments. 

CSPs can be formulated as search problems:

- States - partial assignment
- Successor fn - outputs all states with one new variable assigned
- Goal test - verifies all variables are assigned and all constraints are satisfied

















