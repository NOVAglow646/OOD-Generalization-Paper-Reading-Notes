## OOD泛化相关文章的阅读笔记集合

### 2023

1. FREE LUNCH FOR DOMAIN ADVERSARIAL TRAINING: ENVIRONMENT LABEL SMOOTHING (ICLR2023) 使用简单的domain label smoothing方法来解决DANN训练不稳定的问题，给出了丰富的理论支持 [[paper]](https://arxiv.org/abs/2302.00194)[[论文分享会slide]](./all_notes/2023.2.17domain adversarial training with ELS.pdf)

### 2022

1. Out-of-distribution Generalization with Causal Invariant Transformations (CVPR2022) 想办法直接分出causal和non-causal feature，然后用causal feature训练。本文给出的结果是，causal feature很难找，但是寻找使causal feature保持不变的变换较为容易[[paper]](https://arxiv.org/abs/2203.11528)[[note]](./all_notes/Out-of-distribution Generalization with Causal Invariant Transformations.pdf)
2. Exact Feature Distribution Matching for Arbitrary Style Transfer and Domain Generalization (CVPR2022) 通过对齐内容图片和风格图片的高阶统计量，来实现更好的风格迁移，并用迁移的图片来增强泛化能力 [[paper]](https://arxiv.org/abs/2203.07740) [[论文分享会slide]](./all_notes/2022.11.17-EHM(final).pdf)

### 2021

1. The Many Faces of Robustness: A Critical Analysis of OOD Generalization (ICCV2021) OOD robustness不应该是一个简单的指标，它在不同distribution shift下应该是不同的指标。难以推断现有方法中哪个能更广泛地work[[paper]](https://openaccess.thecvf.com/content/ICCV2021/papers/Hendrycks_The_Many_Faces_of_Robustness_A_Critical_Analysis_of_Out-of-Distribution_ICCV_2021_paper.pdf)[[note]](./all_notes/The Many Faces of Robustness A Critical Analysis of OOD Generalization.pdf)



### 经典文章

1. Domain-Adversarial Training of Neural Networks (2016) 提出DANN，训练domain label判别器来促使特征提取器学习到domain-invariant的特征 [[paper]](https://www.jmlr.org/papers/volume17/15-239/15-239.pdf)

