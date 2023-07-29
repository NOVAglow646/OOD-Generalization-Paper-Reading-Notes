### **[ICLR 2022]** [Handling Distribution Shifts on Graphs: An Invariance Perspective](https://link.zhihu.com/?target=https%3A//openreview.net/forum%3Fid%3DFQOC5u-1egI) : EERM

node-level task，**分类每一个node。**

只有一个graph，一个domain。

#### graph ood的难点

1. 由于互连，node之间不是iid的
2. 除了节点特征外，图结构也有信息，需要利用

#### method

**节点级建模**：将图分解为一系列子图的（may overlap）的集合$\left\{\left(G_v, y_v\right)\right\}_{v \in V}$

**OOD problem:** $\min _f \max _{e \in \mathcal{E}} \mathbb{E}_{G \sim p(\mathbf{G} \mid \mathbf{e}=e)}\left[\frac{1}{|V|} \sum_{v \in V} \mathbb{E}_{y \sim p\left(\mathbf{y} \mid \mathbf{G}_{\mathbf{v}}=G_v, \mathbf{e}=e\right)}\left[l\left(f\left(G_v\right), y\right)\right]\right]$



##### 具体优化目标

$$\min _\theta \mathbb{V}_{\mathbf{e}}\left[L\left(G^e, Y^e ; \theta\right)\right]+\beta \mathbb{E}_{\mathbf{e}}\left[L\left(G^e, Y^e ; \theta\right)\right]$$

第一项：loss方差最小化，希望各个环境loss接近，类似V-REx

先通过一个AT method ，进行删/增边和最大化每个子图的方差来获得k个子图（来模拟k个环境），然后再min每个子图（环境）上loss的variance和分类loss。

具体优化目标如下：

$\begin{aligned}
& \min _\theta \operatorname{Var}\left(\left\{L\left(g_{w_k^*}(G), Y ; \theta\right): 1 \leq k \leq K\right\}\right)+\frac{\beta}{K} \sum_{k=1}^K L\left(g_{w_k^*}(G), Y ; \theta\right) \\
& \text { s. t. }\left[w_1^*, \cdots, w_K^*\right]=\arg \max _{w_1, \cdots, w_K} \operatorname{Var}\left(\left\{L\left(g_{w_k}(G), Y ; \theta\right): 1 \leq k \leq K\right\}\right)
\end{aligned}$

#### blog

https://zhuanlan.zhihu.com/p/580112987

