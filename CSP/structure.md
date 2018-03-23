### Structure

Consider a tree-structured CSP (no loops in constraint graph), we can reduce the runtime for finding a solution from $$O(d^N)$$ to $$O(Nd^2)$$.

**Idea**:

- Pick a random node in the constraint graph as the root of the tree.
- Convert all undirected edges in the tree to directed edges that point away from the root. Then linearize (or topologically sort) the directed acyclic graph.
- Perform a backwards pass of arc consistency, enforce arc consistency for all arcs $$\text{Parent}(i) \longrightarrow i$$
- Perform a forward assignment. Because we have enforced arc consistency on all of the arcs, the children will each have at least one consistent value (proved by induction). The iterative assignment guarantees a correct solution.

##### Cutset Conditioning

First find the smallest subset of variables such that their removal results in a tree (a cutset for the graph). Assign all variables in it and prune the domains of all neighboring nodes. Then solve the resulting tree-structured CSP(s).

Let $$c$$ be the size of the cutset, the runtime will be $$O(d^c(n-c)d^2)$$, where $$c$$ is small.