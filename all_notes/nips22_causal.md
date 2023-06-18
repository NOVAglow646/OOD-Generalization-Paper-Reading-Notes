### [NeurIPS 2022] Learning Causally Invariant Representations for Out-of-Distribution Generalization on Graphs

**总的来说，**首先通过g来预测causal子图，其补图就是spurious subgraph。然后通过推导说明了causal feature与环境无关的条件可以转化为最大化同一类别的feature的互信息（通过一个supervised contrastive loss实现）。在这个约束下，再最大化 $G_s$  $G_c$  分别与y的互信息（$G_s$ 是 $G_c$  的补图，最大化Gs和y的互信息是为了“将Gs从Gc中分离出去”），同时要保证Gs与y的互信息比Gc和y的互信息要小（互信息的优化通过优化loss实现，loss越小互信息越大）。

