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
