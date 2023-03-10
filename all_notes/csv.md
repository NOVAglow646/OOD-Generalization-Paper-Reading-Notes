**核心：**$f(X)$独立于$Z$，given $Y$，是OOD泛化的充分条件。

局限：解决的是correlation shift。他对correlation shift的定义：

Definition 1. Given training distribution $P$, the test distribution $Q \in \mathcal{P}$ has **correlation shift**, where
$$
\mathcal{P}=\left\{Q_{X, Y, Z}: Q_Y=P_Y, Q_{X \mid Y, Z}=P_{X \mid Y, Z}\right\} .
$$




$Q_{X \mid Y, Z}=P_{X \mid Y, Z}$对于diersity shift不成立。因为P和Q的Z的support都不一样。【能否扩展到diversity shift？】



提出的指标CSV：$\operatorname{CSV}(f)=\mathbb{E}_Y\left[\sup _{z_1, z_2}\left(\mathbb{E}_X\left[\mathcal{L}(f(X), Y) \mid Y, Z=z_1\right]-\mathbb{E}_X\left[\mathcal{L}(f(X), Y) \mid Y, Z=z_2\right]\right)\right]$

**是given y时，不同spurious feature的loss的差距。**

疑问：

1. 