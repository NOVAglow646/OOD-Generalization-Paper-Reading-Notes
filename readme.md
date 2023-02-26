## OOD Generalization

### 2023

1. **FREE LUNCH FOR DOMAIN ADVERSARIAL TRAINING: ENVIRONMENT LABEL SMOOTHING** (ICLR2023) 使用简单的domain label smoothing方法来解决DANN训练不稳定的问题，给出了丰富的理论支持 [[paper]](https://arxiv.org/abs/2302.00194)[[论文分享会slide]](./all_notes/2023.2.17domain_adversarial_training_with_ELS.pdf)
2. **Generalizability of Adversarial Robustness Under Distribution Shifts** (AAAI2023) AT有助于Domain Adaptation，但是在Domain Generalization setting下不如正常模型 [[paper]](https://arxiv.org/abs/2209.15042)
3.  **WHAT IS MISSING IN IRM TRAINING AND EVALUATION? CHALLENGES AND SOLUTIONS** (ICLR2023) 指出large-batch会使得各IRM方法的结果不准，small-batch的效果足以匹敌基于large-batch的优化算法。指出了CMNIST只测-90%是不完善的。基于IRM-GAME提出了BLOC-IRM，发现显示地引入per-environment stationary正则项能缓解各domain分类器一致的这个约束带来的各环境分类器次优的问题。[[paper]](https://openreview.net/forum?id=MjsDeTcDEy)
4. **Aggregation of Disentanglement: Reconsidering Domain Variations in Domain Generalization** (arxiv2023) 使用contrastive learning来拉近统一domain数据的表示、推远不同domain数据的表示（文章claim自己是首个在contrastive learning for DG中考虑不同domain的），同时提出所谓的domain-specific feature也有助于泛化（但是文章的实现算法貌似没有显式的解耦invariant/variant feature的过程，也没有显示地构建domain-specific feature）[[paper]](https://arxiv.org/abs/2302.02350)

### 2022

1. **Out-of-distribution Generalization with Causal Invariant Transformations** (CVPR2022) 想办法直接分出causal和non-causal feature，然后用causal feature训练。本文给出的结果是，causal feature很难找，但是寻找使causal feature保持不变的变换较为容易[[paper]](https://arxiv.org/abs/2203.11528)[[note]](./all_notes/Out-of-distribution_Generalization_with_Causal_Invariant_Transformations.pdf)
2. **Exact Feature Distribution Matching for Arbitrary Style Transfer and Domain Generalization** (CVPR2022) 对AdaIN的改进，通过对齐内容图片和风格图片的高阶统计量，来实现更好的风格迁移，并用迁移的图片来增强泛化能力 [[paper]](https://arxiv.org/abs/2203.07740) [[论文分享会slide]](./all_notes/2022.11.17-EHM(final).pdf)
3. **Enhance the Visual Representation via Discrete Adversarial Training** (NeurIPS 2022) 利用VQGAN，在其产生的离散表示上做AT，生成具有更少高频噪声、更大范数、更符合现实世界OOD偏移的augmented data，来增强OOD泛化。刷爆许多ImageNet OOD dataset的SOTA。 [[paper]](https://arxiv.org/abs/2209.07735) [[论文分享会slide]](./all_notes/2022.12.15DAT.pdf)
4. **Pyramid Adversarial Training Improves ViT Performance** (CVPR2022) 通过具有不同尺度结构扰动的AT来增强ViT性能，学到的扰动能保存shape信息 [[paper]](https://arxiv.org/abs/2111.15121)
5. **CROSSMATCH: CROSS-CLASSIFIER CONSISTENCY REGULARIZATION FOR OPEN-SET SINGLE DOMAIN GENERALIZATION** (ICLR2022) 很难的新setting，open-set single domain generalization [[paper]](https://openreview.net/forum?id=48RBsJwGkJf) [[notes]](./all_notes/cross-matching-ICLR22.pdf)
6. **UNCERTAINTY MODELING FOR OUT-OF-DISTRIBUTION GENERALIZATION** (ICLR2022) 假设数据的均值和方差服从潜在的高斯分布 $\mathcal{N}(\mu,\Sigma_\mu)$ 和 $\mathcal{N}(\sigma,\Sigma_\sigma)$ ，提出了估计 $\Sigma_\mu$ 和 $\Sigma_\sigma$ 的方法，并由此提出了一种新的数据增强 [[paper]](https://arxiv.org/abs/2202.03958) [[notes]](./all_notes/uncertain.pdf)
7. **Gradient Matching for Domain Generalization** （ICLR2022）通过最大化不同environment的loss梯度的内积，来寻找domain invariant features。[[paper]](https://openreview.net/forum?id=vDwBW49HmO) [[notes]](./all_notes/grad_fishr.pdf)
8. **Fishr: Invariant Gradient Variances for Out-of-distribution Generalization** (ICML2022) match不同domain的loss梯度的协方差矩阵 [[paper]](https://arxiv.org/abs/2109.02934) [[notes]](./all_notes/grad_fishr.pdf)
9. **Towards Principled Disentanglement for Domain Generalization** (CVPR2022 Oral) 分别使用两个网络建模semantic特征和variation特征（non-causal），并通过这两个特征重建x，要求重建过程关于variation特征不变，提出了所谓的*Invariance based on disentanglement*。 [[paper]](https://arxiv.org/abs/2111.13839)
10. **On the Strong Correlation Between Model Invariance and Generalization** (NeurIPS2022) 提出了新的不变性衡量标准EI（衡量的是网络对于x和经OOD变换后的x'的预测的差距），发现其对于不变性的刻画远优于JS divergence。同时验证了EI与OOD泛化性能的正比关系。[[paper]](https://arxiv.org/pdf/2207.07065.pdf)

### 2021

1. **The Many Faces of Robustness: A Critical Analysis of OOD Generalization** (ICCV2021) OOD robustness不应该是一个简单的指标，它在不同distribution shift下应该是不同的指标。难以推断现有方法中哪个能更广泛地work[[paper]](https://openaccess.thecvf.com/content/ICCV2021/papers/Hendrycks_The_Many_Faces_of_Robustness_A_Critical_Analysis_of_Out-of-Distribution_ICCV_2021_paper.pdf)[[note]](./all_notes/The-Many-Faces-of-Robustness-A-Critical-Analysis-of-OOD-Generalization.pdf)
2. **Improved OOD Generalization via Adversarial Training and Pre-training** (ICML2021) 从理论上证明了在用Wasserstein距离刻画分布偏移的情况下，AT对一定距离内的OOD数据有泛化能力 [[paper]](https://arxiv.org/abs/2105.11144) 
3. **Learning Representations that Support Robust Transfer of Predictors** (Arxiv2021) 提出了TRM(Transfer Risk Minimization)，把模型在不同domain的泛化能力直接作为objective优化 [[paper]](https://arxiv.org/abs/2110.09940) [[notes]](./all_notes/TRM.pdf)
4. **Learning Causal Semantic Representation for OoD prediction** (NeurIPS2021) 很硬核，提出了一种新的Causal Semantic Model，文章关键假设是 $p(x|s,v)$ 和 $p(y|s)$ 在domain间保持不变， $p(s,v)$ 是domain change的唯一来源。证明了causal机制的可辨识性  [[paper]](https://proceedings.neurips.cc/paper/2021/file/310614fca8fb8e5491295336298c340f-Paper.pdf) [[notes]](./all_notes/causal1.pdf)
5. **Exploiting Domain-Specifific Features to Enhance Domain Generalization** (NeurIPS2021) 使用了很多trick：从information bottleneck的角度证明了domain-specific feature对泛化也是有帮助的；提出了一种基于最小化domain-invariant feature和domain-specific feature之间协方差矩阵的解耦；提出通过meta learning来利用domain-specific feature的泛化能力。[[paper]](https://arxiv.org/abs/2110.09410) [[notes]](./all_notes/disen1.md)



### 经典文章

1. **Domain-Adversarial Training of Neural Networks** (2016) 提出DANN，训练domain label判别器来促使特征提取器学习到domain-invariant的特征 [[paper]](https://www.jmlr.org/papers/volume17/15-239/15-239.pdf)
2. **In Search of Lost Domain Generalization** (ICLR2021) Domainbed benchmark [[paper]](https://arxiv.org/abs/2007.01434)
3. **Invariant Risk Minimization** (2019) IRM [[paper]](https://arxiv.org/abs/1907.02893)



## Adversarial Robustness

### 2022

1. **The Dimpled Manifold Model of Adversarial Examples in Machine Learning** (arxiv 2022) 从manifold角度研究网络训练的过程以及AT的行为。发现训练DNN大概分为两个过程：①使决策边界由随机快速分布到数据的manifold附近 ②通过在决策边界中产生凹凸，来使决策边界移动到样本的正确一侧。AT会使得决策边界的起伏更大，即向off-manifold方向移动。[[paper]](https://arxiv.org/abs/2106.10151) [[notes]](./all_notes/The-Dimpled-Manifold.pdf)
2. **Understanding Adversarial Robustness Against On-manifold Adversarial Examples** (arxiv 2022) manifold角度研究AT行为，发现了on-manifold对抗样本的存在，提出了在GAN的隐空间做AT、在训练数据的特征向量张成的子空间中做AT两种思路 [[paper]] (https://arxiv.org/abs/2210.00430) [[notes]](./all_notes/Understanding-Adversarial-Robustness-Against.pdf)



## 其他杂文

1. **On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima** (ICLR2017) large-batch会导致模型更容易进入尖锐的极小值点，而尖锐的极小值点会不利于泛化 [[paper]](https://arxiv.org/abs/1609.04836)

