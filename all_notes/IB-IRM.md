“(2)” 回顾一下IRM的data generation的假设

“but is no different from ERM and fails in covariate shift-CMNIST (CS-CMNIST).” IRM会在covariate shift下失效



本文的观察1（fig 2 a所示的SCM）：

“We observe that **when the invariant features are fully informative, both IRM and ERM fail but only in classification tasks and not in regression tasks**”





“Define Inv-Margin = minz∈∪e∈Etr Ze inv sgn(w∗ inv · z)(w∗ inv · z).” 表示特征到决策边界的最近距离

“Theorem 2”，如果invariant feature的支撑集没有overlap（ass7成立），则OOD泛化无法实现。



“Theorem 3” ERM和IRM的failure：当invariant features是可分的：

1. 当spurious featureoverlap时，IRM和ERM都能成功，甚至他们的有些使用spurious feature的解也能成功
2. 当spurious feature不overlap时，ERM和IRM都失败



IB的作用：

“Observe that all the classifiers in S achieve a zero classification error on the training environments. However, only classifiers for which wspu = 0 solve the OOD generalization (eq. (1)).” 比较有意思的是，**同时使用inv和sp会导致熵更大，而单独用哪个都不会导致熵更大。**



“Why invariance?” 当spurious feature的与Y的关联的噪声比invariant feature小（注意：这个噪声表征了特征被反转的概率，bernoulli(p)的p越大说明被反转的概率越大，即用这个特征预测Y越不稳定）（虽然spurious feature的噪声在每个环境都不一样，但每个环境的噪声都比invariant feature的小），IB会使用sp



“Why invariance and information bottleneck?”



“information bottleneck constraints on top ensure that representations that only use Xe inv are used.” 因为虽然只用inv和只用sp熵一样，但是只用inv分类误差小。IB保证了只用inv或只用sp而非两者都用，此时进一步最小化训练误差就能只用inv