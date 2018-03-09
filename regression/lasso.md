### Lasso Regression

**Regression fn**: $$h(x,w) = w^{\top}x$$

**Loss fn**: $$L(z, y) = (z-y)^2$$

**Cost fn**: $$J(h) = \frac{1}{n}\sum_{i=1}^{n} L(h(x_i), y) + \lambda \|w\|_1$$

**Optimization problem**: $$\min\limits_{w} \sum_{i=1}^{n} (w^{\top}x_i-y_i)^2 + \lambda\|w\|_1$$

全称是least absolute shrinkage and selection operator。与ridge regression相比，$$\|w\|^2_2$$的isosurface是hypersphere，而$$\|w\|_1$$的isosurface是cross-polytope，导致lasso中objective的导数是连续不光滑的，不能够通过求导并用gradient descent求解。但由cross-polytope的几何性质可以看出，lasso可以将一部分系数缩减到$$0$$，实现特征筛选。

