### Least-Squares Linear Regression

**Regression fn**: $$h(x, w, a) = w^{\top}x$$

**Loss fn**: $$L(z, y) = (z-y)^2$$

**Cost fn**: $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y)$$

我们将所有的$$x_i$$写入一个$$n*(d+1)$$的矩阵$$X$$（design matrix）。矩阵的每一行表示一个input，每一列表示一个feature。用一个维度为$$d+1$$的向量$$w$$表示weight。用一个维度为$$n$$的向量$$y$$表示输入数据对应的response的值。

**Optimization problem**: $$\min\limits_{w} \sum_{i=1}^{n} (w^{\top}x_i-y_i)^2$$

现在可以将该最优化问题改写为 $$\min\limits_{w} \|Xw-y\|_2^2$$，这个问题相当于最小化$$RSS(w)$$。

$$\begin{align*} RSS(w) &= (Xw-y)^{\top}(Xw-y) \\&= w^{\top}X^{\top}Xw - 2y^{\top}Xw + y^{\top}y\end{align*}$$

对w求导，可知

$$ \begin{align*} \nabla RSS(w) &= 2X^{\top}Xw - 2X^{\top}y = 0 \\ X^{\top}Xw &= X^{\top}y\end{align*} $$

当$$X^{\top}X$$可逆时，可知$$w = (X^{\top}X)^{-1}X^{\top}y$$  
当$$X^{\top}X$$为singular时，相当于$$n$$小于$$d+1$$，不能够通过$$n$$个等式求解$$d+1$$个未知数。

##### Interpretation as a Projection

$$\hat{y} = Xw$$ 表明$$\hat{y}$$是$$X$$所有列的linear combination （$$X$$的每一列表示了一个feature）。为了最小化$$RSS(w)$$，实际上就是要找到一个$$w$$使$$y-\hat{y}$$垂直于$$X$$的column space。根据垂直条件我们可以写出$$X^{\top}(Xw-y) = 0$$，和求导的结果一致。注意到$$\hat{y} = X(X^{\top}X)^{-1}X^{\top}y$$， 其中$$X(X^{\top}X)^{-1}X^{\top}$$被称为projection matrix，也叫做hat matrix。

##### Pros and Cons

优势：容易计算，当$$X^{\top}X$$可逆的时候有唯一解  
劣势：因为对误差进行了平方，所以对outlier比较敏感。同时当$$X^{\top}X$$不可逆时不能使用这个方法。

