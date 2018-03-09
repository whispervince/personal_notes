### Introduction

回归，可以理解为fitting curves to data。 对于输入的$$x$$（一般是一个表示feature的向量），输出一个值（而不是直接给出一个表示类别的整数）。建立模型的过程可以分成两个部分。

1. 选取一个regression fn, $$h(x, p)$$。常用的有

   1. linear，可以表示为$$h(x, w, a) = w^{\top}x+a$$, $$w$$表示weight vector，$$w_i$$是对应第$$i$$个feature的系数。

      可以将常数项$$a$$写到$$w$$中，并且在$$x$$中加入一项$$1$$，通过增加一个维度将方程的形式简化为$$h(x,w) = w^{\top}x$$（之后都采用这个形式分析）

   2. polynomial，$$h(w,x) = w_0 + w_1x + w_2x^2 + \ldots w_dx^d $$

   3. logistic，$$h(x, w) = s(w^{\top}x)$$, $$s(r) = \frac{1}{1+e^{-r}} $$。logistic fn的range在0到1之间，可以用来当做概率来理解。

2. 选取衡量预测好坏的loss fn。常见的loss fn有：

   1. squared error，$$L(z, y) = (z-y)^2$$ （z表示预测，y表示真实值）

   2. absolute error， $$L(z, y) = |z-y| $$

   3. logistic loss， $$L(z, y) = -y\ln z-(1-y)\ln (1-z)$$（也叫做cross entropy）

3. 选取衡量对于整个数据集的预测的好坏的cost fn。常用的cost fn有：

   1. mean loss， $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) $$，注意对于一个数据集来说$$n$$可以视为常数。

   2. max loss，$$J(h) = \max\limits_{i=1, \ldots, n} L(h(x_i), y) $$

   3. weighted sum， $$ J(h) = \sum_{i=1}^{n} w_iL(h(x_i), y) $$

   4. $$l_2$$ penalized/regularized，$$ J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) + \lambda \|w\|_2^2$$

   5. $$l_1$$ penalized/regularized，$$ J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) + \lambda \|w\|_1$$

通过不同的fn的组合可以得到一些经典的，或者比较好求解，或者比较好解释的模型，常见的如下

1. least-squares linear regression，\(1\) + \(1\) + \(1\)
2. weighted least-squares linear regression，\(1\) + \(1\) + \(3\)
3. ridge regression，\(1\) + \(1\) + \(4\)
4. lasso，\(1\) + \(1\) + \(5\)
5. logistic regression，\(3\) + \(3\) + \(1\)
6. least absolute deviations，\(1\) + \(2\) + \(1\)
7. Chebyshev criterion，\(1\) + \(2\) + \(2\)

其中，前三个都属于quadratic program，可以通过一定的变换，通过求导直接minimize。lasso属于quadratic program但是没有解析解。logistic是convex的，可以通过gradient descent求解。最后两个是linear problem。

