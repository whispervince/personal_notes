### Weighted Least-Squares Regression

**Regression fn**: $$h(x, w) = w^{\top}x$$

**Loss fn**: $$L(z, y) = (z-y)^2$$

**Cost fn**: $$J(h) = \sum_{i=1}^{n} w_iL(h(x_i), y)$$

在least-squares linear regression的基础上，给每一个input（不是每一个feature）设置一个权重$$w_i$$。将所有的$$w_i$$写成一个$$n*n$$的矩阵$$\Omega$$，$$\Omega_{ii} = w_i$$，$$\Omega_{ij} = 0 \ \forall i \neq j$$。

**Optimization problem**:  $$\min\limits_{w} \sum_{i=1}^{n} w_i(w^{\top}x_i-y_i)^2$$ （$$w$$仍然表示weight vector，并且已经包含了常数项$$a$$）

该最优化问题可以写为 $$\min\limits_{w} (Xw-y)^{\top}\Omega(Xw-y)$$，可以通过求导直接找到最小值。

$$\begin{align*}\nabla_w (Xw-y)^{\top}\Omega(Xw-y) &= 2X^{\top}\Omega Xw - 2X^{\top}\Omega y = 0 \\ X^{\top}\Omega Xw &= X^{\top}\Omega y\end{align*}$$

类比在linear regression里面的推导，我们可以写出

$$ \begin{align*}\hat{y} &= X(X^{\top}\Omega X)^{-1}X^{\top}\Omega y \\ \Omega^{\frac{1}{2}}\hat{y} &= \Omega^{\frac{1}{2}}X(X^{\top}\Omega X)^{-1}X^{\top}\Omega y  \end{align*}$$

注意到$$\Omega^{\frac{1}{2}} \hat{y}$$是$$\Omega^{\frac{1}{2}} y$$在$$\Omega^{\frac{1}{2}} X$$的column space上的投影。

