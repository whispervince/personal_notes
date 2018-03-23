### Introduction

A search problem is formulated by:

- State space - the set of all possible states in the given world
- Successor fn - a fn that takes in a state and an action, and computes the cost of the action and the successor states
- Start state - the initial state of the agent
- Goal test - a fn that takes in a state and determines that whether it is a goal state

A search problem is solved by starting from the start state and iteratively exploring the state space by calling the successor fn until reaching the goal state, at which point a path is determined.

##### State Space Graphs

Constructed with states representing nodes, with directed edges existing from a state to its successors. These edges represent actions, and any associated weights represent the cost of performing the corresponding action. In a graph, each state is represented exactly once.

##### Search Trees

Each node encodes the entire path from the start state to the given state in the state space graph.

No restriction on the number of times a state can appear. Greater than or equal to their corresponding state space graph in size.