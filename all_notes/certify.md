1. **B.2 Analyses of DG using distributional divergence** 包含从source和target域分布距离的角度给出泛化误差的工作，在推导泛化误差时可以参考
2. fig 1 给出的信息：OOD偏移类型不同时，**相同距离的分布偏移的empirical loss也会差别很大**。所以本文主张优化某一个距离下的worst case loss。
3. section 3.2给出了一种计算worst case loss的可行的方式，即改为优化一个surrogate function