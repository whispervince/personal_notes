### Logistic Regression

**Regression fn**: $$h(x, w) = s(w^{\top}x)$$，$$s(r) = \frac{1}{1+e^{-r}}$$

由于$$s(\cdot)$$的range为$$(0, 1)$$，回归方程的通常值情况下可以理解为一个sample point属于一个类别的概率。同时，输入的$$y_i$$也可以是概率，但在很多情况下$$y_i = 0$$ 或$$y_i=1$$。

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

在$v$点附近，$$\nabla J$$的泰勒级数为

$$\nabla J(w) = \nabla J(v) + \nabla^2 J(v)(w-v) + O(|w-v|^2)$$

其中$$\nabla^2J(v)$$是$$J(\cdot)$$在$v$点的Hessian matrix。令$$\nabla J(w) = 0$$，可得

$$\begin{align*} \nabla J(w) &= 0 \\ \nabla J(v) + \nabla^2 J(v)(w-v) &= 0 \\ \nabla^2 J(v)(w-v) &= -\nabla J(v)\end{align*}$$

注意到并不需要计算逆矩阵，可以通过求解该线性方程得到新的$$w$$。每一层循环中解$$\nabla^2 J(w)e = -\nabla J(w)$$，可以得到$$w' = w + e$$。

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

$$\nabla^2J(w) = \sum_{i=1}^{n}s_i(1-s_i)x_ix_i^{\top} = X^{\top}\Omega X$$ ，$$\Omega$$是一个对角矩阵，$$\Omega_{i,i} = s_i(1-s_i)$$

此时可以采用Newton's method，每一层求解方程$$X^{\top}\Omega Xe = X^{\top}(y-s(Xw))$$，得到$$w' = w + e$$。

可以注意到每一层循环中求解的方程和weighted least-squares有相似的形式，但是每一层的$\Omega$都不尽相同。这一类问题被称为iteratively reweighted least-squares。每一层循环中会考虑离decision boundary距离较近的点，然后用这些点修改decision boundary。相当于每一层都只考虑了一个较小的subset，以此提高效率。

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

