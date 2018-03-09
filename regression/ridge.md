### Ridge Regression

**Regression fn**: $$h(x, w) = w^{\top}x$$

**Loss fn**: $$L(z, y) = (z-y)^2$$

**Cost fn**: $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) + \lambda \|w\|_2^2$$

**Optimization problem**: $$\min\limits_{w} \sum_{i=1}^{n} (w^{\top}x_i-y_i)^2 + \lambda\|w\|_2^2$$

该最优化问题可以用用矩阵的形式表示为

$$\begin{align*}&\min\limits_{w} \|Xw-y\|_2^2 + \lambda\|w\|_2^2 \\ \Leftrightarrow & \min\limits_{w}(Xw-y)^{\top}(Xw-y) + \lambda w^{\top}w \\ \Leftrightarrow &\min\limits_{w} w^{\top}X^{\top}Xw - 2y^{\top}Xw + y^{\top}y + \lambda w^{\top}w\end{align*}$$

对$$w$$求导，将其导数设为$$0$$可以得到

$$\begin{align*} \nabla_w (w^{\top}X^{\top}Xw - 2y^{\top}Xw + y^{\top}y + \lambda w^{\top}w) &= 2X^{\top}Xw - 2X^{\top}y + 2\lambda Iw = 0  \\ (X^{\top}X + \lambda I)w &=X^{\top}y \end{align*} $$

该方程也展示了ridge regression的一个作用，通过加入$$l_2$$-regularization（$$\lambda$$ &gt; 0），$$X^{\top}X + \lambda I$$总是可逆矩阵，所以一定存在一个解。ridge regression解决了least-squares不能处理的input的multicolinearity的问题。

##### $$l_2$$ regularization

regularization term，也称为penalty term，作用有以下几点

1. 保证$$X^{\top}X + \lambda I$$为可逆矩阵，从而保证有唯一的解

   首先我们可以证明$$X^{\top}X$$是positive semi-definite的

   $$v^{\top}X^{\top}Xv = \|Xv\|_2^2 \ge 0 \quad \forall v$$

   但是positive semi-definite不能保证矩阵可逆。此时如果引入regularization，当$$\lambda > 0$$时，可以看到

   $$v^{\top}(X^{\top}X + \lambda I)v = \|Xv\|_2^2 + \lambda v^{\top}v > 0 \quad \forall v \neq 0$$

   可以得到$$X^{\top}X + \lambda I \succ 0$$

   根据positive definite的性质可知，$$X^{\top}X + \lambda I$$的所有eigenvalue都大于0，等价于该矩阵可逆

2. 通过惩罚太大的weight，可以减少overfit。当weight过大的时候，input的微小的改变会导致回归方程的值有很大的变化。所以我们希望能有较小的weight，从而获得更简单的模型。最后得到的结果是会选取更多的特征，并且相应的weight会接近于0。

3. 和linear regression相比，引入regularization会带来一定的bias，但是可以减小variance。使模型更稳定。

   由

   $$\begin{align*} \mathbb{E} (h(z)) - f(z) &= \mathbb{E}(w^{\top}z) - v^{\top}z\\ &=\mathbb{E}((X^{\top}X + \lambda I)^{-1}X^{\top}(Xv + e)) - v^{\top}z \\&=\mathbb{E}((X^{\top}X + \lambda I)^{-1}X^{\top}Xv) - v^{\top}z\end{align*}$$

   可知引入regularization增加了bias。但同时可以发现 当$$\lambda \rightarrow \infty$$时，$$\text{Var}(z^{\top}(X^{\top}X+\lambda I)^{-1}X^{\top}e) \rightarrow 0$$。可以通过交叉验证来找到一个较好的$$\lambda$$的值。

4. \*Grouping effect，相关性强的一组feature会有相近的weights？

##### Bayesian Justification

从Bayes theorem的角度出发，Ridge regression的模型也可以看做是假设先验概率$$w \sim N(0, \sigma^2)$$ （$w$越接近0可能性越大），然后通过maximize a posteriori（MAP）导出。由贝叶斯可写出后验概率：

**Posterior**: $$P(w|X, y) = \frac{P(y|w)P(w)}{P(X,y)}$$

**Log posterior**: $$\ln P(y|w) + \ln P(w) - \ln P(X,y)$$

其中$$y|w$$也服从正态分布，$$\ln P(y|w)$$可以写成$$-c\|Xw-y\|^2$$，所以问题相当于

$$\max\limits_{w} -c_1\|Xw-y\|^2-c_2\|w\|_2^2 - c_3,\quad c_1,c_2,c_3\text{ some constant}$$

等价于

$$\min\limits_{w} \|Xw -y\|_2^2 + \lambda \|w\|_2^2$$

##### Remark

通常情况下，为了使所有的feature受到同样的penalty，要预先将feature normalize，使所有的feature有相同的variance。也可以对每个feature设置不同的$$\lambda$$，将上面的$$I_{d+1}$$替换为一个对角矩阵即可。

