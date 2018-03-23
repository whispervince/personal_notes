### Least-Squares Polynomial Regression

**Regression fn**: $$h(x, w) = w_0 + w_1x + w_2x^2 + \ldots + w_px^p$$

对于任何一个$$x_i$$，将其替换为一个向量。举例来说，当$$p=2$$，$$x \in \mathbb{R}^2$$时，我们把$$x_i$$替换为

$$\Phi(x_i) = \begin{bmatrix}x_{i1}^2 & x_{i1}x_{i2} & x_{i2}^2 &x_{i1} &x_{i2} & 1\end{bmatrix}^{\top}$$

然后就可以当成least-squares linear regression求解。除了多项式以外也可以使用其他形式的feature。当$$p$$较大时很容易overfit。

