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

### Lasso Regression

**Regression fn**: $$h(x,w) = w^{\top}x$$

**Loss fn**: $$L(z, y) = (z-y)^2$$

**Cost fn**: $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) + \lambda \|w\|_1$$

**Optimization problem**: $$\min\limits_{w} \sum_{i=1}^{n} (w^{\top}x_i-y_i)^2 + \lambda\|w\|_1$$

全称是least absolute shrinkage and selection operator。与ridge regression相比，$$\|w\|^2_2$$的isosurface是hypersphere，而$$\|w\|_1$$的isosurface是cross-polytope，导致lasso中objective的导数是连续不光滑的，不能够通过求导并用gradient descent求解。但由cross-polytope的几何性质可以看出，lasso可以将一部分系数缩减到$$0$$，实现特征筛选。

### Logistic Regression

**Regression fn**: $$h(x, w) = s(w^{\top}x)$，$s(r) = \frac{1}{1+e^{-r}}$$

由于$$s(\cdot)$$的range为$$(0, 1)$$，回归方程的通常值情况下可以理解为一个sample point属于一个类别的概率。同时，输入的$y\_i$也可以是概率，但在很多情况下$$y_i = 0$$ 或$$y_i=1$$。

**Loss fn**: $$L(z, y) = -y\ln z-(1-y)\ln (1-z)$$

**Cost fn**: $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y)$$

**Optimization problem**: $$\min\limits_{w} -\sum_{i=1}^{n}y_i\ln s(w^{\top}x_i)+(1-y_i)\ln (1-s(w^{\top}x_i))$$

尽管logistic regression没有解析解（采用IRLS计算，Iteratively Reweighted Least Squares），但objective仍然是convex的，可以通过gradient descent求解。

考虑$$s(\cdot)$$的导数

$$\begin{align*} s'(r) &= \frac{d}{dr}\frac{1}{1+e^{-r}} \\ &=\frac{e^{-r}}{(1+e^{-r})^2} \\&= s(r)(1-s(r))\end{align*}$$

令$$s_i = s(w^{\top}x_i)$$，将objective对$$w$$求导

$$\begin{align*} \nabla_w J &= -\sum_{i=1}^{n}\frac{y_i}{s_i}\nabla_ws_i - \frac{1-y_i}{1-s_i}\nabla_ws_i\\ &=  -\sum_{i=1}^{n}(\frac{y_i}{s_i} - \frac{1-y_i}{1-s_i})\nabla_ws_i \\ &=  -\sum_{i=1}^{n}(\frac{y_i}{s_i} - \frac{1-y_i}{1-s_i})s_i(1-s_i)x_i \\ &= -\sum_{i=1}^{n}(y_i-s_i)x_i \\&= -X^{\top}(y-s(Xw))\end{align*}$$

所以gradient descent的迭代关系为$$w \leftarrow w + \epsilon X^{\top}(y-s(Xw))$$。

##### Newton's Method

该方法是针对smooth的cost fn常用的iterative optimization method。通常情况下比gradient descent要快。

**Idea**: 在任何一点$$v$$，用一个quadratic fn来近似表示该点附近的cost fn，然后直接移动到该二次函数导数为$0$的位置。重复以上操作直到获得一个满意的结果。

在$$v$$点附近，$$\nabla J$$的泰勒级数为

$$\nabla J(w) = \nabla J(v) + \nabla^2 J(v)(w-v) + O(|w-v|^2)$$

其中$$\nabla^2J(v)$$是$$J(\cdot)$$在$$v$$点的Hessian matrix。令$$\nabla J(w) = 0$$，可得

$$\begin{align*} \nabla J(w) &= 0 \\ \nabla J(v) + \nabla^2 J(v)(w-v) &= 0 \\ \nabla^2 J(v)(w-v) &= -\nabla J(v)\end{align*}$$

注意到并不需要计算逆矩阵，可以通过求解该线性方程得到新的$w$。每一层循环中解$$\nabla^2 J(w)e = -\nabla J(w)$$，可以得到$$w' = w + e$$。

和gradient descent比较有以下优缺点：

**Advantage**:

1. Newton's method能找到一个更优的descent的方向。
2. gradient descent中步长是人为定义的，而Newton's method能找到能到达minimum的正确的步长。
3. 如果使用的cost fn是quadratic的，Newton's method一步找到最小值。

**Disadvantage**:

1. 不知道导数为$0$的点是最大值，最小值，还是鞍点。
2. 需要从一个靠近最小值的点出发才能找到正确的解。
3. 计算Hessian的成本太高。
4. 只有当cost fn是smooth的时候才能使用。（利用了泰勒级数）

根据这些特性，通常情况下可以用gradient descent靠近最小值，然后切换到Newton's method。

在logistic regression中，已经推导到

$$\nabla_w J= -\sum_{i=1}^{n}(y_i-s_i)x_i  = -X^{\top}(y-s(Xw))$$

可以进一步写出

$$\nabla^2J(w) = \sum_{i=1}^{n}s_i(1-s_i)x_ix_i^{\top} = X^{\top}\Omega X$$ ，$$\Omega$$是一个对角矩阵, $$\Omega_{i,i} = s_i(1-s_i)$$

此时可以采用Newton's method，每一层求解方程$$X^{\top}\Omega Xe = X^{\top}(y-s(Xw))$$，得到$$w' = w + e$$。

可以注意到每一层循环中求解的方程和weighted least-squares有相似的形式，但是每一层的$$\Omega$$都不尽相同。这一类问题被称为iteratively reweighted least-squares。每一层循环中会考虑离decision boundary距离较近的点，然后用这些点修改decision boundary。相当于每一层都只考虑了一个较小的subset，以此提高效率。

##### LDA vs. Logistic Regression

和QDA/LDA不同的是，logistic regression属于discriminant model。不需要将数据fit到一个具体的分布中，而是直接fit到logistic fn中。

**LDA的优势**：

1. 对于类别是well-separated的数据，LDA很稳定。
2. 在有超过两个类别的情况下，LDA更加简单。而logistic regression需要修改。
3. 当数据接近正态分布时，LDA的准确率稍微高一些。

**Logistic regression的优势**：

1. 更加强调decision boundary，对outlier比较不敏感。
2. 能够给出属于某个类别的概率，而LDA只能给出是否属于某个类别。
3. 对于不满足正态分布的数据更加稳定。

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

### Subset Selection

由于所有的feature都会带来variance，但不是所有的feature都能减少bias，所以希望能够剔除掉不好的feature。同时，通过选择更少的feature，可以获得更加简单的模型，更加好解释。

**Algorithm**:

Best subset selection， 假设一共有$$d$$个feature，用所有$$2^d-1$$个非空子集建立模型，通过交叉验证找到最好的一个子集。

很明显，这个算法虽然能够找到最优解，但是复杂度过于恐怖。一般采用另外的近似算法。

**Heuristic 1**:

Forward stepwise selection

1. 从$$0$$个feature开始，不断加入当前最优的feature直到test error开始增加。
2. 每层循环，通过validation选择一个当前最优的feature

最多选取$$d$$个feature，每次循环要检验$$O(d)$$个feature，一共需要训练$$O(d^2)$$个模型，远少于$$O(2^d)$$。但是不能保证是最优解。

**Heuristic 2**:

Backward stepwise selection

1. 从$$d$$个feature开始，每次移除一个让test error能够减少最多的feature。
2. 每层循环，通过validation选择一个当前最优的feature。

**Additional heuristic**: 只移除对应的weight比较小的feature。考虑到由least-squares得到的variance和$$\sigma^2(X^{\top}X)^{-1}$$成正比，再次根据我不明白的计算，可知$$w_i$$对应的z-score，$$z_i = \frac{w_i}{\sigma\sqrt{v_i}}$$，$$v_i$$是$$(X^{\top}X)^{-1}$$主对角线上的第$$i$$项。当$$w_i$$较小时，会有较小的z-score，说明$$w_i$$的真实值可能是$$0$$。

