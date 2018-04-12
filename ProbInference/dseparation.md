### Bayes Nets - D-Separation

The conditional independence problem can be formulated as: given a Bayes Net $$G$$, two nodes $$X$$ and $$Y$$, and a set of nodes $$\{Z_1,…Z_k\}$$ that represent observed variables, must the following statement be true: $$X \bot Y|\{Z_1,… Z_k\}$$?

**Causal chain**:

$$X \longrightarrow Y \longrightarrow Z, P(x,y,z) = P(z|y)P(y|x)P(x)$$.

$$X$$ and $$Z$$ are not guaranteed to be independent. But it is true that $$X \bot Z | Y$$, which is equivalent to $$P(X|Z,Y) = P(X|Y)$$.

$$\begin{align*} P(X|Z, y) &= \frac{P(X,Z,y)}{P(X,y)} \\ &= \frac{P(Z|y)P(y|X)P(X)}{\sum_x P(x, y, Z)} \\ &= \frac{P(Z|y)P(y|X)P(X)}{P(Z|y)\sum_x P(y|x)P(x)} \\ &= \frac{P(y|X)P(X)}{\sum_x P(y|x)P(x)} \\&= \frac{P(y|X)P(X)}{P(y)} \\&= P(X|y) \end{align*}$$

**Common cause**:

$$X \longleftarrow Y \longrightarrow Z, P(x,y,z) = P(x|y)P(z|y)P(y)$$

$$X$$ and $$ Z$$ are not guaranteed to be independent. But it is true that $$X \bot Z | Y$$, which is equivalent to $$P(X|Z,Y) = P(X|Y)$$.

$$\begin{align*} P(X|Z, y) &= \frac{P(X|y)P(Z|y)P(y)}{P(Z|y)P(y)} \\ &= P(X|y) \end{align*}$$

**Common effect**:

$$X \longrightarrow Y \longleftarrow Z, P(x,y,z) = P(y|x,z)P(x)P(z)$$

$$X$$ and $$Z$$ are independent, but they are not necessarily independent when conditioned on $$Y$$. $$X$$ and $$Z$$ are not guaranteed to be independent when conditioning on descendents of $$Y$$.

**D-separation algorithm**:

1. Shade all observed nodes.
2. Enumerate all undirected path from $$X$$ to $$Y$$.
3. For each path
1. Decompose the path into triples.
2. If all triples are active, this path is action and d-connects $$X$$ and $$Y$$.
4. If no path d-connects $$X$$ and $$Y$$, then they are d-separated, which means that they are conditionally independent given the observations.

**Active triples**:

- Causal chain without observation
- Common cause without observation
- Common effect with observation
- Common effect with observation of descendent of the effect

**Inactive triples**:

- Causal chain with observation (blocked)
- Common cause with observation
- Common effect without observation