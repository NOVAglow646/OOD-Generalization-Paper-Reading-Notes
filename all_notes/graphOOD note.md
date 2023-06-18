### **[NeurIPS 2022 Spotlight] Learning Substructure Invariance for Out-of-Distribution Molecular Representations**

图分类。

没有环境标签。



#### 总体目标

$\max _{\omega, \Phi} \mathrm{I}(\mathbf{z} ; \mathbf{y}) \text {, s.t. } \min _{\omega, \Phi} \mathrm{I}(\mathbf{y} ; \mathbf{e} \mid \mathbf{z})$

即 $\max _{q_\theta(\mathbf{y} \mid \mathbf{z}), q_\theta(\mathbf{z} \mid \mathbf{G})} \mathrm{I}(\mathbf{z} ; \mathbf{y}) \text {, s.t. } \min _{q_\theta(\mathbf{y} \mid \mathbf{z}), q_\theta(\mathbf{z} \mid \mathbf{G})} \mathrm{I}(\mathbf{y} ; \mathbf{e} \mid \mathbf{z}) ~~~~~~~(1)$, 

max项表示希望z有足够的预测能力，min项表示希望z与环境无关。其中$q_\theta(z|G)$和$q_\theta(y|z)$分别表示encoder $\Phi$和 classifier $w$的输出。

$q_\theta(z|G)$和$q_\theta(y|z)$二者的具体实现：

1. encoder $\Phi$：包括一个molecular encoder和substructure encoder，它们二者的分别得到整个分子的表示和子结构的表示，之后在通过一个attentive pooling，拿着molecular encoder的输出当query去找substructure encoder的输出
2. classifier $w$ ：一个MLP.

直接优化上述目标是不可处理的。文章对此的解决是先推了一个ELBO用来优化得到一个环境划分器来获得环境标签e，然后证明了$(1)$可以用如下的目标来具体优化。

![image-20230531165821354](C:\Users\19000\AppData\Roaming\Typora\typora-user-images\image-20230531165821354.png)

①是invariance项 ②是sufficiency项

文章证明了：

1. min ① 等于 $\min _{q_\theta(\mathbf{y} \mid \mathbf{z}), q_\theta(\mathbf{z} \mid \mathbf{G})} \mathrm{I}(\mathbf{y} ; \mathbf{e} \mid \mathbf{z})$，也就是让$z$在所有测试环境表现一样，即$p(y|z,e)=p(y|z)$
2. min ② 等于 $\max _{q_\theta(\mathbf{y} \mid \mathbf{z}), q_\theta(\mathbf{z} \mid \mathbf{G})} \mathrm{I}(\mathbf{z} ; \mathbf{y})$，也就是让$z$有足够的预测能力





#### 环境划分器

通过最大化如下的ELBO（等于是在最小化环境分类器和真实后验分布的KL散度$D_{K L}\left(q_\kappa(e \mid G, y) \| p(e \mid G)\right)$）:

$\mathcal{L}_{\text {elbo }}(\tau, \kappa ; \mathcal{G})=\frac{1}{|\mathcal{G}|} \sum_{(G, y) \in \mathcal{G}}\left[\mathbb{E}_{q_\kappa}\left[\log p_\tau(y \mid G, e)\right]-D_{K L}\left(q_\kappa(e \mid G, y) \| p(e \mid G)\right)\right]$

来训练环境划分器$q_k(e|G,y)$ 和 conditioned-GNN $p_\tau(y|G,e) $

具体实现：

环境分类器 $q_k(e|G,y)$： Graph Isomorphism Network (GIN)，接受一个图和标签，产生环境的概率分布

先验分布 $p_\tau(e|G)$：均匀分布或高斯分布

条件GNN $p_\tau(y|G,e) $ ：一个GIN

<img src="C:\Users\19000\AppData\Roaming\Typora\typora-user-images\image-20230531165919608.png" alt="image-20230531165919608" style="zoom:50%;" />

#### 训练过程

<img src="C:\Users\19000\AppData\Roaming\Typora\typora-user-images\image-20230617151159992.png" alt="image-20230617151159992" style="zoom:50%;" />





