### Statistical Justification for Regression

##### Least-Squares Regression from Max Likelihood

**Model**: $$y_i = f(x_i) + \epsilon_i, \epsilon_i \sim N(0, \sigma^2), \forall x_i$$

**Log likelihood**:

$$\begin{align*} &\ell(f, y_i) = -\frac{|y_i-f(x_i)|^2}{2\sigma^2} - c, c \text{ some constant} \\ \Rightarrow \quad & \ell(f,y) = -\frac{1}{2\sigma^2}\sum_{i=1}^{n}|y_i-f(x_i)|^2- c', c' \text{ some constant}\end{align*}$$

所以通过least-squares，也就是相当于最小化$$\sum_{i=1}^{n}|y_i-f(x_i)|^2$$，可以得到让likelihood最大的$$f(\cdot)$$。

##### Logistic Regression from Max Likelihood

**Model**:

$$y_i$$表示$$x_i$$属于这个类别的实际概率，$$h(x_i)$$表示预测的概率。

假设每一个$$x_i$$我们都有$$\beta$$个duplicates，则可以说有$$\beta y_i$$个$$x_i$$属于这个类别，剩下的$$\beta (1-y_i)$$个不属于这个类别。

**Likelihood**: $$\mathcal{L}(h) = \prod_{i=1}^{n}h(x_i)^{\beta y_i}(1-h(x_i))^{\beta (1-y_i)}$$

**Log likelihood**:

$$\begin{align*}\ell(h) &= \sum_{i=1}^{n}\beta y_i \ln h(x_i) + \beta(1-y_i)\ln (1-h(x_i)) \\&= -\beta\sum_{i=1}^{n}-y_ih(x_i)-(1-y_i)\ln(1-h(x_i))\end{align*}$$

所以通过最小化logistic loss的和，可以获得让likelihood最大的$$h(\cdot)$$。

##### Bias-Variance Decomposition

一个假设的error可以分为以下两种：

1. Bias，估计值的期望和真实值的差距。
2. Variance，估计值的离散程度，也就是估计值距离期望值的距离。

假设$$y_i = f(x_i) + \epsilon, \ \mathbb{E}(\epsilon) = 0, \ \text{Var}(\epsilon) = \sigma^2$$，采用的模型是$$h(\cdot)$$。采用squared error作为loss fn，可以写出

$$\begin{align*}\mathbb{E}((h(x_i)-y_i)^2) &= \mathbb{E}(h(x_i)^2) + \mathbb{E}(y_i^2) -2\mathbb{E}(y_ih(x_i)) \\ &= \text{Var}(h(x_i)) + \mathbb{E}(h(x_i))^2 + \text{Var}(y_i) + \mathbb{E}(y_i)^2-2\mathbb{E}(h(x_i))\mathbb{E}(y_i) \\ &=(\mathbb{E}(h(x_i)) - \mathbb{E}(y_i))^2 + \text{Var}(h(x_i)) + \sigma^2\end{align*} $$

这里$$\mathbb{E}(h(x_i)) - \mathbb{E}(y_i)$$是模型的bias，$$\text{Var}(h(x_i))$$是模型的variance，$$\sigma^2$$是irreducible error。

##### Bias-Variance Notes

1. irreducible error不能被减少。
2. 如果一个模型underfit，则会有较大的bias。
3. \*variance过大会导致overfit。？
4. training error能够反应bias但是不能反应variance；test error则能反应两种。
5. 如果选择的模型能很好的fit真实值，对于大部分分布来说当$n$趋向于无穷时bias趋向于$$0$$。
6. 如果选择的模型不能很好的fit真实值，则在大部分位置都会有很大的bias。
7. 对于大部分分布来说当$n$趋向于无穷时variance趋向于$$0$$。
8. 加入一个好的feature能够减少bias，而加入一个不好的feature并不怎么会增加bias。但是引入新的feature经常会增加variance。所以除非新加入的feature带来的variance小于消除掉的bias，不要随意添加feature。
9. training set中的noise会影响bias和variance，而test set中的noise只会影响irreducible error。
10. 通常情况下并不知道数据和noise的真实的分布，但可以人为选择一些分布来测试算法并尝试找到较好一些的模型。

##### Bias-Variance Example - Least-Sqaure Linear Regression

**Model**: 假设真实值为$$f(z) = v^{\top}z$$，noise为$$e$$，$$e_i \sim N(0, \sigma^2)$$。则training value可以写成$$y = Xv  + e$$。

由least-squares linear regression可知

$$w = (X^{\top}X)^{-1}X^{\top}y =  (X^{\top}X)^{-1}X^{\top}(Xv+e) = v + (X^{\top}X)^{-1}X^{\top}e$$。

**Bias**:

$$\begin{align*} \mathbb{E} (h(z)) - f(z) &= \mathbb{E}(w^{\top}z) - v^{\top}z\\ &=\mathbb{E}(v^{\top}z + z^{\top}(X^{\top}X)^{-1}X^{\top}e) - v^{\top}z \\&=z^{\top}(X^{\top}X)^{-1}X^{\top}\mathbb{E} (e) \\ &= 0\end{align*}$$

当bias为0时，有可能找到一个完美的fit。

**Variance**:

$$\begin{align*} \text{Var}(h(z)) &= \text{Var}(w^{\top}z) \\ &= \text{Var}(v^{\top}z + z^{\top}(X^{\top}X)^{-1}X^{\top}e)\\ &= \text{Var}(z^{\top}(X^{\top}X)^{-1}X^{\top}e) \\&=z^{\top}(X^{\top}X)^{-1}X^{\top}\sigma^2 I(z^{\top}(X^{\top}X)^{-1}X^{\top})^{\top}\\&=\sigma^2z^{\top}(X^{\top}X)^{-1}z\end{align*}$$

根据我没想明白的计算，可以写出$$\text{Var}(h(z)) \approx \sigma^2\frac{d}{n}$$，其中$$d$$表示$$z$$的维度，即每一个sample point所含feature的数量。可以注意到variance随着feature数量增加而增加，随sample的增加而减小。

