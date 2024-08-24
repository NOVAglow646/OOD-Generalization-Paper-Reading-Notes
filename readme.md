## Preface

本仓库记录关于OOD Generalization (Domain Generlization)/Graph OOD Generalization/In-context Learning/LLM/LVM等topic的文章。看过的文章会至少用一句话概括内容，有些还会有notes。只有标题的就是还没看过的，只是先存档到这里。

###  🔥 Updates

- 2024-05 接下来主要关注探究ICL机制的相关工作
- 2024-02-21 接下来将会主要关注LLM/Multi-modal LLMs的generalization。

## Directory

* [OOD Generalization of (Multimodal) LLMs](#ood-generalization-of-multimodal-LLMs)
* [OOD Generalization using Large Vision-language models](#ood-generalization-using-large-vision-language-models)
* [New tasks of OOD Generalization](#new-tasks-of-ood-generalization)
* ⭐[OOD Generalization (classic)](#ood-generalization-classic)
* [Test-time Adaptation for Generalization](#test-time-adaptation-for-generalization)
* [Domain Generalization/Adaptation in Down-stream CV/NLP Tasks](#domain-generalizationadaptation-in-down-stream-cvnlp-tasks)
* ⭐[Graph OOD Generalization (graph-level & node-level)](#graph-ood-generalization-graph-level--node-level)
* [Domain Adaptation](#domain-adaptation)
* [LLMs/Large Multi-modal models](#llmslarge-multi-modal-models) 
* [Prompt Learning](#prompt-learning)
* ⭐[In-Context Learning](#in-context-learning)
* [ICL Theories](#icl-theories)
* [Others](#others)
    * [Optimization](#optimization) 
    * [Adversarial Robustness](#adversarial-robustness)
    * [Neural Collapse](#neural-collapse)
    * [Feature Collpase](#feature-collapse)
    * [Causality](#causality)
    * [Graph Homophily/Heterophily](graph-homophilyheterophily)
* [Slides](#slides)



## OOD Generalization of (Multimodal) LLMs 

### 2024

1. **On the Out-Of-Distribution Generalization of Multimodal Large Language Models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.06599) 对MLLM进行了一系列OOD性能的验证，发现：①在经典OOD数据集上MLLM能超过以前的sota ②在医学、分子数据集上效果不好 ③ICL能提升MLLM OOD性能，即使ICL example的分布和测试域不同。

### 2023

1. **In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax** (Arxiv Nov 2023) [[paper] ](https://arxiv.org/abs/2311.07811) 本文通过构建一些语法任务来测试模型对于句子结构的理解能力，以及OOD泛化性能。总的说来，LLM还是会用到一些spurious correlation。
2. **On the Robustness of ChatGPT: An Adversarial and Out-of-distribution Perspective** (ICLR 2024 workshop highlighted) [[paper]](http://arxiv.org/abs/2302.12095)
3. **Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes** (Arxiv Nov 2023, ongoing work) [[paper]](Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes) 发现模型对于demonstration的permutation invariance或许是ICL OOD的关键。提出使用相同的positional encoding来提升ICL OOD性能。
4. **A Closer Look at In-Context Learning under Distribution Shifts** (Arxiv May 2023) [[paper]](http://arxiv.org/abs/2305.16704) 在一定的covariate shift下，transformer比set-based MLP的性能好；在严重的分布偏移下，两种模型的ICL能力都丧失了。3. 4. 这两篇都是整个测试prompt和训练prompt分布不同的情况。
5. **Few-shot Fine-tuning vs. In-context Learning: A Fair Comparison and Evaluation** (Arxiv May 2023) [[paper]](https://www.lsv.uni-saarland.de/wp-content/uploads/2023/07/Few-shot-Fine-tuning-vs.-In-context-Learning.pdf) 在参数量相当的情况下，ICL的OOD不如FT。30B的ICL跟6.7B的FT性能相当。大部分情况下ICL不如FT。

### 2022

1. **What Can Transformers Learn In-Context? A Case Study of Simple Function Classes** (NeurIPS 2022) [[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/hash/c529dba08a146ea8d6cf715ae8930cbe-Abstract-Conference.html) 在一个toy linear regression任务上，尝试了两种OOD setting：①test和train prompt分布不同 ②test时的ICE和query分布不同。发现ICL在这两种场景下均有一定的泛化能力。



## OOD Generalization using Large Vision-language models

### 2023

1. **PromptStyler: Prompt-driven Style Generation for Source-free Domain Generalization** (ICCV 2023) 先训练K个用于生成style word embedding（多样性+保持语义），然后把它们和N个类别词embedding结合，喂给CLIPtext生成K*N个style-content的text feature。之后拿这些feature训练一个linear classifier。推断时把这个classifier接到CLIP image encoder上（**因为在joint image-text空间中，存在cross-modal transferability phenomenon**）。 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Cho_PromptStyler_Prompt-driven_Style_Generation_for_Source-free_Domain_Generalization_ICCV_2023_paper.html)
3. **A Sentence Speaks a Thousand Images: Domain Generalization through Distilling CLIP with Language Guidance** (ICCV 2023) 将CLIP的language encoder输出的embedding作为"generic text representation"，然后让student（visual model）的表示去对齐teacher（CLIP）的text representation。同时对齐student和teacher的预测logit。【insight 1】recent work发现基于环境划分的方法不那么work，因为真实世界的环境划分不明确 【insight 2】从优化难度角度说明anchor sample在对齐时的作用 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Huang_A_Sentence_Speaks_a_Thousand_Images_Domain_Generalization_through_Distilling_ICCV_2023_paper.html) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
4. **Distilling Out-of-Distribution Robustness from Vision-Language Foundation Models** (NIPS 2023) 在用In domain数据生成的Discrete adversarial eaxmple上拿一个CLIP做蒸馏提升就能超过普通的Knowledge Distillation和DAT [[paper]](https://arxiv.org/pdf/2311.01441.pdf) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
5. **Distilling from Vision-Language Models for Improved OOD Generalization in Vision Tasks** (Arxiv Oct 2023) 对齐student model经过一个projector后的表示和CLIP的text/image encoder的输出 [[paper]](https://arxiv.org/abs/2310.08255) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
6. **Context-Aware Robust Fine-Tuning** (IJCV Dec 2023) 发现微调会破坏CLIP原有的对于context（虚假特征）zero-shot的分类能力。提出在微调的时候对齐预训练的CLIP预测的context的概率分布和正在被微调的模型预测的context的概率分布 [[paper]](https://link.springer.com/article/10.1007/s11263-023-01951-2) 
7. **ArGue: Attribute-Guided Prompt Tuning for Vision-Language Models**(Arxiv Nov 2023) [[paper]](http://arxiv.org/abs/2311.16494) 本文提出了neg prompt，就是在prompt里加入neg attribute的embedding。在prompt tuning时，希望在使用neg prompt时，预测成任何类别的概率相同，以此来强行消除虚假关联（文中loss式(9)）。使用neg prompt时预测为某一类的概率就是用neg embedding+任何class的embedding和图像表示算相似度。



## New tasks of OOD Generalization

1. **SimMMDG: A Simple and Effective Framework for Multi-modal Domain Generalization** (NeurIPS 2023) 多模态DG，将各模态的特征分为不同模态share的部分以及各个模态specific的部分，拉近同一class的不同模态shared部分的特征，推远share部分的特征和specific部分的特征。[[paper]](https://openreview.net/forum?id=RiSMijlsLT)



## OOD Generalization (classic)
### 2024
1. **Spurious Feature Diversification Improves Out-of-distribution Generalization** (ICLR 2024) 通过ensemble学更多的spurious feature能“冲淡”它们各自的影响 [[paper]](https://openreview.net/forum?id=d6H4RBi7RH)
1. **Out-Of-Domain Unlabeled Data Improves Generalization** (ICLR 2024 spotlight) [[paper]](https://openreview.net/forum?id=Bo6GpQ3B9a)
1. **Robust agents learn causal world models** (ICLR 2024 Oral) [[paper]](http://arxiv.org/abs/2402.10877) 通过构建一个causal influence diagram (CID，一种基于causal baysian network扩展的模型)，证明了对于一个决策任务（分类、回归等传统任务都可以用决策任务来建模），学习近似的causal mnodel是学到误差有界的策略的充要条件。
1. **Context is Environment** (ICLR 2024) [[paper]](https://openreview.net/forum?id=8VPWfqtQMX) 提出以一种ICL的范式来帮助domain generalization， 本质上是一种test-time adaptation
1. **Ask Your Distribution Shift if Pre-Training is Right for You** (ICLR 2024 rejected) 【结论存疑】实验上发现pretrain对out-of-support数据（类似diversity shift）更有用，对in support（类似correlation shift）作用不大。
1. **Improving Domain Generalization with Domain Relations** (ICLR 2024 spotlight) [[paper]](https://openreview.net/forum?id=Dc4rXq3HIA) 利用数据集的meta-data构建domain relation，在训练时优化这个relation并为每个domain训练一个domain specific head，在测试时根据test和training domain之间的relation加权每个training domain expert的预测作为最终预测。

### 2023

1. **FREE LUNCH FOR DOMAIN ADVERSARIAL TRAINING: ENVIRONMENT LABEL SMOOTHING** (ICLR2023) 使用简单的domain label smoothing方法来解决DANN训练不稳定的问题，给出了丰富的理论支持 [[paper]](https://arxiv.org/abs/2302.00194)[[论文分享会slides]](./all_notes/2023.2.17domain_adversarial_training_with_ELS.pdf)

2. **Generalizability of Adversarial Robustness Under Distribution Shifts** (AAAI2023) AT有助于Domain Adaptation，但是在Domain Generalization setting下不如正常模型 [[paper]](https://arxiv.org/abs/2209.15042)

3. **WHAT IS MISSING IN IRM TRAINING AND EVALUATION? CHALLENGES AND SOLUTIONS** (ICLR2023) 指出large-batch会使得各IRM方法的结果不准，small-batch的效果足以匹敌基于large-batch的优化算法。指出了CMNIST只测-90%是不完善的。基于IRM-GAME提出了BLOC-IRM，发现显示地引入per-environment stationary正则项能缓解各domain分类器一致的这个约束带来的各环境分类器次优的问题。[[paper]](https://openreview.net/forum?id=MjsDeTcDEy)

4. **Aggregation of Disentanglement: Reconsidering Domain Variations in Domain Generalization** (Arxiv 2023) 使用contrastive learning来拉近统一domain数据的表示、推远不同domain数据的表示（文章claim自己是首个在contrastive learning for DG中考虑不同domain的），同时提出所谓的domain-specific feature也有助于泛化（但是文章的实现算法貌似没有显式的解耦invariant/variant feature的过程，也没有显示地构建domain-specific feature）[[paper]](https://arxiv.org/abs/2302.02350)

5. **Domain Generalization Emerges from Dreaming** (arxiv2023) 先通过类似AdaIN的方法生成风格迁移的图像 $x'$，再minimize原始图像 $x$ 和 $x'$的prediction的差距。性能甚至可以超过SWAD。[[paper]](https://arxiv.org/pdf/2302.00980.pdf)

6. **BREAKING CORRELATION SHIFT VIA CONDITIONAL INVARIANT REGULARIZER** (ICLR2023) 证明了对于correlation shift，在给定y时，若网络输出 $f(X)$ 独立于spurious feature $Z$ ，则可保证OOD [[paper]](https://arxiv.org/pdf/2207.06687.pdf)

7. **ASSESSING MODEL OUT-OF-DISTRIBUTION GENERALIZATION WITH SOFTMAX PREDICTION PROBABILITY BASELINES AND A CORRELATION METHOD** (ICLR 2023 rejected) 提出了新的OOD metric，鼓励：①预测的confident高 ②预测具有多样性（避免所有样本被预测为同一个类）。因为被审稿人喷缺乏理论依据而被拒。[[openreview] ](https://openreview.net/forum?id=1maXoEyeqx)

8. **MODELING THE DATA-GENERATING PROCESS IS NECESSARY FOR OUT-OF-DISTRIBUTION GENERALIZATION** (ICLR 2023 top 25%) 用统一的causal graph来描述correlation shift和diversity shift，提出了multi-shift的问题，并证明了学到的表示必须满足由causal graph所导出的条件独立性条件是保证OOD泛化的必要条件。 [[paper]](https://openreview.net/forum?id=uyqks-LILZX) [[论文分享会slides]](./all_notes/2023.3.15[ICLR2023]CACM.pdf)

9. **Domain Generalization via Nuclear Norm Regularization** (Arxiv 2023) 认为引入对 $\phi(x)$ 的低秩constraint有助于解耦不变特征和环境特征 [[paper]](https://arxiv.org/pdf/2303.07527.pdf)

10. **REVISITING ADAPTERS WITH ADVERSARIAL TRAINING** 发现AdvProp中为clean和adv样本分别准备BN层不是必须的，关键在于共享参数+domain adaptator (ICLR 2023) [[paper]](https://openreview.net/forum?id=HPdxC1THU8T)

11. **UNDERSTANDING OUT-OF-DISTRIBUTION ACCURACIES THROUGH QUANTIFYING DIFFICULTY OF TEST SAMPLES**  (Arxiv 2023) 使用entropy based confusion score来衡量样本的difficulty，发现OOD数据confusion score更高（模型预测的熵更高） [[paper]](https://arxiv.org/pdf/2203.15100.pdf)

12. **Effective Robustness against Natural Distribution Shifts for Models with Different Training Data** (Arxiv 2023) [[paper]](https://arxiv.org/pdf/2302.01381.pdf)

13. **Sharpness-Aware Gradient Matching for Domain Generalization** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Wang_Sharpness-Aware_Gradient_Matching_for_Domain_Generalization_CVPR_2023_paper.html)

14. **Improved Test-Time Adaptation for Domain Generalization** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Chen_Improved_Test-Time_Adaptation_for_Domain_Generalization_CVPR_2023_paper.html)

15. **Domain Generalization Guided by Gradient Signal to Noise Ratio of Parameters** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Michalkiewicz_Domain_Generalization_Guided_by_Gradient_Signal_to_Noise_Ratio_of_ICCV_2023_paper.html)

16. **Cross Contrasting Feature Perturbation for Domain Generalization** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Li_Cross_Contrasting_Feature_Perturbation_for_Domain_Generalization_ICCV_2023_paper.html)

17. **A Sentence Speaks a Thousand Images: Domain Generalization through Distilling CLIP with Language Guidance** (ICCV 2023) 将CLIP的language encoder输出的embedding作为"generic text representation"，然后让student（visual model）的表示去对齐teacher（CLIP）的text representation。同时对齐student和teacher的预测logit。【insight 1】recent work发现基于环境划分的方法不那么work，因为真实世界的环境划分不明确 【insight 2】从优化难度角度说明anchor sample在对齐时的作用 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Huang_A_Sentence_Speaks_a_Thousand_Images_Domain_Generalization_through_Distilling_ICCV_2023_paper.html)

18. **Understanding Hessian Alignment for Domain Generalization** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/papers/Hemati_Understanding_Hessian_Alignment_for_Domain_Generalization_ICCV_2023_paper.pdf)

19. **Flatness-Aware Minimization for Domain Generalization** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Zhang_Flatness-Aware_Minimization_for_Domain_Generalization_ICCV_2023_paper.html)

20. **MAP: Towards Balanced Generalization of IID and OOD through Model-Agnostic Adapters** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Zhang_MAP_Towards_Balanced_Generalization_of_IID_and_OOD_through_Model-Agnostic_ICCV_2023_paper.html)

21. **Learning Diverse Features in Vision Transformers for Improved Generalization** (ICML 2023 Spurious Correlations Workshop) 最小化ViT不同head对于同一token梯度的相似性来鼓励模型的diversity。实验发现ViT的不同token之间有些是spurious，有些是shift-robust的。提出的regularization有助于扩大不同head的gap。 [[paper]](https://arxiv.org/pdf/2210.04206.pdf)

22. **Towards Understanding Feature Learning in Out-of-Distribution Generalization** (NeulPS 2023) ERM已经能够学到inv和sp特征。不好的ERM pretrain会导致IRMv1学不到inv特征。提出了一种迭代学习前一轮没学到的特征的算法FAT。 [[paper]](https://arxiv.org/pdf/2304.11327.pdf)

23. **Diversify and Disambiguate: Learning From Underspecified Data** (ICLR 2023) 很像CVPR2022 Evading the Simplicity Bias: Training a Diverse Set of Models Discovers Solutions With Superior OOD Generalization的做法：同一个backbone（feature extractor）训练很多head，加正则鼓励这些head的不同，然后借助oracle信息来选出一个头（本文是用一个所谓的最informative的测试子集来选，CVPR那篇是用一个ood validation set来选）。[[paper]](http://arxiv.org/abs/2202.03418)

24. **SimMMDG: A Simple and Effective Framework for Multi-modal Domain Generalization** (NeurIPS 2023) 多模态DG，将各模态的特征分为不同模态share的部分以及各个模态specific的部分，拉近同一class的不同模态shared部分的特征，推远share部分的特征和specific部分的特征。[[paper]](https://openreview.net/forum?id=RiSMijlsLT)

25. **PromptStyler: Prompt-driven Style Generation for Source-free Domain Generalization** (ICCV 2023) 先训练K个用于生成style word embedding（多样性+保持语义），然后把它们和N个类别词embedding结合，喂给CLIPtext生成K*N个style-content的text feature。之后拿这些feature训练一个linear classifier。推断时把这个classifier接到CLIP image encoder上（**因为在joint image-text空间中，存在cross-modal transferability phenomenon**）。 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Cho_PromptStyler_Prompt-driven_Style_Generation_for_Source-free_Domain_Generalization_ICCV_2023_paper.html)

26. **Model Ratatouille: Recycling Diverse Models for Out-of-Distribution Generalization** (ICML 2023) 利用很多在不同任务上fine-tune的同一个预训练模型来提升OOD性能 [[paper]](https://proceedings.mlr.press/v202/rame23a/rame23a.pdf) 

27. **Explore and Exploit the Diverse Knowledge in Model Zoo for Domain Generalization** (ICML 2023) 充分利用多样的预训练模型（不仅仅是利用最强的模型）来提升OOD性能 [[paper]](https://proceedings.mlr.press/v202/chen23m/chen23m.pdf)

28. **Understanding and Improving Feature Learning for Out-of-Distribution Generalization** (NeurIPS 2023) [[paper]](https://proceedings.neurips.cc/paper_files/paper/2023/file/d73d5645ddbb9ada6c862116435574f6-Paper-Conference.pdf)

    


### 2022

1. **Out-of-distribution Generalization with Causal Invariant Transformations** (CVPR2022) 给出了一种不需要discover causal feature的方法，而是直接找Causal Invariant Transformation，证明了如果网络输出对于CIT不变，则可实现OOD泛化 [[paper]](https://arxiv.org/abs/2203.11528)[[note]](./all_notes/Out-of-distribution_Generalization_with_Causal_Invariant_Transformations.md)
2. **Exact Feature Distribution Matching for Arbitrary Style Transfer and Domain Generalization** (CVPR2022) 对AdaIN的改进，通过对齐内容图片和风格图片的高阶统计量，来实现更好的风格迁移，并用迁移的图片来增强泛化能力 [[paper]](https://arxiv.org/abs/2203.07740) [[论文分享会slides]](./all_notes/2022.11.17-EHM(final).pdf)
3. **Enhance the Visual Representation via Discrete Adversarial Training** (NeurIPS 2022) 利用VQGAN，在其产生的离散表示上做AT，生成具有更少高频噪声、更大范数、更符合现实世界OOD偏移的augmented data，来增强OOD泛化。刷爆许多ImageNet OOD dataset的SOTA。 [[paper]](https://arxiv.org/abs/2209.07735) [[论文分享会slides]](./all_notes/2022.12.15DAT.pdf)
4. **Pyramid Adversarial Training Improves ViT Performance** (CVPR2022) 通过具有不同尺度结构扰动的AT来增强ViT性能，学到的扰动能保存shape信息 [[paper]](https://arxiv.org/abs/2111.15121)
5. **CROSSMATCH: CROSS-CLASSIFIER CONSISTENCY REGULARIZATION FOR OPEN-SET SINGLE DOMAIN GENERALIZATION** (ICLR2022) 很难的新setting，open-set single domain generalization [[paper]](https://openreview.net/forum?id=48RBsJwGkJf) [[notes]](./all_notes/cross-matching-ICLR22.pdf)
6. **UNCERTAINTY MODELING FOR OUT-OF-DISTRIBUTION GENERALIZATION** (ICLR2022) 假设数据的均值和方差服从潜在的高斯分布 $\mathcal{N}(\mu,\Sigma_\mu)$ 和 $\mathcal{N}(\sigma,\Sigma_\sigma)$ ，提出了估计 $\Sigma_\mu$ 和 $\Sigma_\sigma$ 的方法，并由此提出了一种新的数据增强 [[paper]](https://arxiv.org/abs/2202.03958) [[notes]](./all_notes/uncertain.pdf)
7. **Gradient Matching for Domain Generalization** （ICLR2022）通过最大化不同environment的loss梯度的内积，来寻找domain invariant features。[[paper]](https://openreview.net/forum?id=vDwBW49HmO) [[notes]](./all_notes/grad_fishr.pdf)
8. **Fishr: Invariant Gradient Variances for Out-of-distribution Generalization** (ICML2022) match不同domain的loss梯度的协方差矩阵 [[paper]](https://arxiv.org/abs/2109.02934) [[notes]](./all_notes/grad_fishr.pdf)
9. **Towards Principled Disentanglement for Domain Generalization** (CVPR2022 Oral) 分别使用两个网络建模semantic特征和variation特征（non-causal），并通过这两个特征重建x，要求重建过程关于variation特征不变，提出了所谓的*Invariance based on disentanglement*。 [[paper]](https://arxiv.org/abs/2111.13839)
10. **On the Strong Correlation Between Model Invariance and Generalization** (NeurIPS2022) 提出了新的不变性衡量标准EI（衡量的是网络对于x和经OOD变换后的x'的预测的差距），发现其对于不变性的刻画远优于JS divergence。同时验证了EI与OOD泛化性能的正比关系。[[paper]](https://arxiv.org/pdf/2207.07065.pdf)
11. **OoD-Bench: Quantifying and Understanding Two Dimensions of Out-of-Distribution Generalization** (CVPR2022 Oral) diversity shift/Correlation shift [[paper]](https://arxiv.org/abs/2106.03721) [[notes(包含关于两种OOD数据集所导出的 P(X) 以及 P(Y|X)) 性质的总结]](./all_noets/oodbench.pdf)
12. **Assaying Out-Of-Distribution Generalization in Transfer Learning** (Arxiv 2022) 各种metric对OOD acc影响的实验性总结 [[paper]](https://arxiv.org/abs/2207.09239)
13. **On Certifying and Improving Generalization to Unseen Domains** (Arxiv 2022) 发现了模型对于在同一分布距离下不同类型OOD shift的性能有较大差异，提出了minimize某一距离下worst-case loss。 [[paper]](https://arxiv.org/abs/2206.12364) [[notes]](./all_notes/certify.md)
14. **ZooD: Exploiting Model Zoo for Out-of-Distribution Generalization** 提出了一种OOD accuracy的metric：从多个预训练的特征提取器中中根据MLE选出使似然$p(y'|\phi',y,\phi)$和$p(\phi'|\phi)$最大的模型。在domainbed有大幅提升。[[paper]](https://arxiv.org/pdf/2210.09236.pdf)
15. **ID AND OOD PERFORMANCE ARE SOMETIMES INVERSELY CORRELATED ON REAL-WORLD DATASETS** (Arxiv 2022) 发现了之前工作中忽略的OOD和ID accuray呈反相关的情况（文章claim这是由于diversity-inducing method不够多、不同模型的表现在同一training epoch是不同的而不应该混在一起、不是所有的pretrain seed都会出现而引起） [[https://arxiv.org/pdf/2209.00613.pdf]]
16. **Agreement-on-the-Line: Predicting the Performance of Neural Networks under Distribution Shift** (NeurIPS 2022) 在可以获取OOD unlabelled data的情况下，发现ID和OOD accuracy有强正相关 $\leftrightarrow$ ID和OOD的agreement强正相关 [[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/file/7a8d388b7a17df480856dff1cc079b08-Paper-Conference.pdf)
17. **When Does Group Invariant Learning Survive Spurious Correlations?** (NeurIPS 2022) 指出了group invariant learning必须满足的两个准则
18. **Evading the Simplicity Bias: Training a Diverse Set of Models Discovers Solutions with Superior OOD Generalization** (CVPR 2022) 主张使用多个分类器，并鼓励这些分类器输出对输入特征的梯度差异尽量大，以得到diverse的模型。之后依赖于OOD validation 选出一个最好的模型作为最终测试的模型。 [[paper]](https://ieeexplore.ieee.org/document/9878716/) 
19. **Rich Feature Construction for the Optimization-Generalization Dilemma** (ICML 2022) ERM pre-train对于提升OOD objectivve的性能很关键 [[paper]](https://proceedings.mlr.press/v162/zhang22u/zhang22u.pdf)
20. **Diverse Weight Averaging for Out-of-Distribution Generalization** (DiWA) (NeurIPS 2022) ensembling，平均不同超参的模型 [[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/file/46108d807b50ad4144eb353b5d0e8851-Paper-Conference.pdf)
21. **Ensemble of Averages: Improving Model Selection and Boosting Performance in Domain Generalization** (EoA) (NeurIPS 2022) 先对每个模型做沿训练轨迹的moving average（不同epoch），再对这些moving average得到的模型做ensembling（不同超参）。具体做法：simply combines all checkpoints uniformly starting from batch 100 until the end of training [[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/file/372cb7805eaccb2b7eed641271a30eec-Paper-Conference.pdf)
22. **Model Agnostic Sample Reweighting for Out-of-Distribution Learning** (MAPLE) (ICML 2022) 提出了一个迭代进行的bilevel optimization过程：(1)优化任意的一个OOD loss更新模型参数$\theta^*$ (2)在更新得到的$\theta^*$下优化ERM loss，更新weight $w$
23. 

### 2021

1. **The Many Faces of Robustness: A Critical Analysis of OOD Generalization** (ICCV 2021) OOD robustness不应该是一个简单的指标，它在不同distribution shift下应该是不同的指标。难以推断现有方法中哪个能更广泛地work[[paper]](https://openaccess.thecvf.com/content/ICCV2021/papers/Hendrycks_The_Many_Faces_of_Robustness_A_Critical_Analysis_of_Out-of-Distribution_ICCV_2021_paper.pdf)[[note]](./all_notes/The-Many-Faces-of-Robustness-A-Critical-Analysis-of-OOD-Generalization.pdf)
2. **Improved OOD Generalization via Adversarial Training and Pre-training** (ICML 2021) 从理论上证明了在用Wasserstein距离刻画分布偏移的情况下，AT对一定距离内的OOD数据有泛化能力 [[paper]](https://arxiv.org/abs/2105.11144) 
3. **Learning Representations that Support Robust Transfer of Predictors** (Arxiv 2021) 提出了TRM(Transfer Risk Minimization)，把模型在不同domain的泛化能力直接作为objective优化 [[paper]](https://arxiv.org/abs/2110.09940) [[notes]](./all_notes/TRM.pdf)
4. **Learning Causal Semantic Representation for OoD prediction** (NeurIPS 2021) 很硬核，提出了一种新的Causal Semantic Model，文章关键假设是 $p(x|s,v)$ 和 $p(y|s)$ 在domain间保持不变， $p(s,v)$ 是domain change的唯一来源。证明了causal机制的可辨识性  [[paper]](https://proceedings.neurips.cc/paper/2021/file/310614fca8fb8e5491295336298c340f-Paper.pdf) [[notes]](./all_notes/causal1.pdf)
5. **Exploiting Domain-Specifific Features to Enhance Domain Generalization** (NeurIPS 2021) 使用了很多trick：从information bottleneck的角度证明了domain-specific feature对泛化也是有帮助的；提出了一种基于最小化domain-invariant feature和domain-specific feature之间协方差矩阵的解耦；提出通过meta learning来利用domain-specific feature的泛化能力。[[paper]](https://arxiv.org/abs/2110.09410) [[notes]](./all_notes/disen1.md)
6. **Towards a Theoretical Framework of Out-of-Distribution Generalization** (NeurIPS 2021) 通过定义了conditioned on class的表示不变性，以及表示的informativeness，并据此定义了什么是可学习的OOD问题，推导出了OOD泛化误差上界可以被这两个量表示 [[paper]](https://proceedings.neurips.cc/paper/2021/hash/c5c1cb0bebd56ae38817b251ad72bedb-Abstract.html) 
7. **Quantifying and Improving Transferability in Domain Generalization** (NeurIPS 2021)  提出了基于给定假设族的域间可迁移性的指标（越小越容易迁移/泛化），并提出了一个min-max优化过程来提升迁移性（有理论保证） [[paper]](https://arxiv.org/abs/2106.03632)
8. **SWAD: Domain Generalization by Seeking Flat Minima** (NeurIPS 2021) 提出了SWAD（平均同一模型的不同epoch的参数），并从理论上证明了flat的loss landscape对OOD泛化有帮助 [[paper]](https://arxiv.org/pdf/2102.08604.pdf)
9. **Domain Generalization Using Causal Matching** (ICML 2021) 对齐不同环境的同一类别的表示来学习不变特征。[[paper]](http://arxiv.org/abs/2006.07500)
10. **Invariance Principle Meets Information Bottleneck for Out-of-Distribution Generalization** (Arxiv 2021) 提出了IB-IRM，即加了一个最小化模型表示的熵的目标。本文的一个核心观察是：当Y和spurious feature没有关联（本文称作FIIF，fully informative invariant features，对应于covariate shift的情况）时，普通IRM会失效；在文中构建的FIIF example下，只使用inv feature或只使用sp feature都会导致表示的熵比同时使用inv和sp feature的表示的熵更低，因此IB的目标会导致只使用inv或只使用sp的解，在此基础上最小化分类loss，就能学到仅依赖于inv的解。 [[paper]](http://arxiv.org/abs/2106.06607) [[notes]](./all_notes/IB-IRM.md)
11. 

### 2020

1. **Out-of-Distribution Generalization via Risk Extrapolation (V-REx)** (2020) 目标：

   $\min _{\omega, \Phi} \sum^e \mathcal{R}^e(\omega, \Phi)+\lambda \text{Var}\left(\mathcal{R}^e(\omega, \Phi)\right)$，动机：如果$\Phi$是不变表示，那么不同环境e应该满足 $P^e(Y \mid \Phi(X))=P(Y \mid \Phi(X))$ ，此时loss也应该一样，所以对loss的方差做正则。（个人觉得存在问题，因为当不同环境loss的值一样时，最优分类器也不一定一样）[[paper]](https://arxiv.org/pdf/2003.00688.pdf)

2. **DISTRIBUTIONALLY ROBUST NEURAL NETWORKS FOR GROUP SHIFTS: ON THE IMPORTANCE OF REGULARIZATION FOR WORST-CASE GENERALIZATION (Group DRO)** (ICLR 2020) 将数据集分成(spurious feature, label)的group，最小化每个group上的loss的最大的凸组合（权重可以优化，loss更大的group的loss倾向于有更大的权重） [[paper]](https://arxiv.org/abs/1911.08731)

3. **AUGMIX: A SIMPLE DATA PROCESSING METHOD TO IMPROVE ROBUSTNESS AND UNCERTAINTY** (ICLR 2020) 鼓励模型对于经过不同data augmentation增强后的样本的预测一致性 [[paper]](https://arxiv.org/pdf/1912.02781.pdf)

4. **Adversarial Examples Improve Image Recognition** (NeurIPS 2020)  Advprop [[paper]](https://openaccess.thecvf.com/content_CVPR_2020/html/Xie_Adversarial_Examples_Improve_Image_Recognition_CVPR_2020_paper.html)

5. **Improving out-of-distribution generalization via multi-task self-supervised pretraining** (Arxiv 2020) SSL pretraining tasks for OOD [[paper]](http://arxiv.org/abs/2003.13525)

### 经典文章

1. **Domain-Adversarial Training of Neural Networks** (2016) 提出DANN，训练domain label判别器来促使特征提取器学习到domain-invariant的特征 [[paper]](https://www.jmlr.org/papers/volume17/15-239/15-239.pdf)
2. **In Search of Lost Domain Generalization** (ICLR 2021) Domainbed benchmark [[paper]](https://arxiv.org/abs/2007.01434)
3. **Invariant Risk Minimization** (2019) IRM 注意：IRM的泛化误差bound中，X是通过虚假特征和不变特征concatenate再经过线性变换得来的，很strict [[paper]](https://arxiv.org/abs/1907.02893)
4. **Self-Challenging Improves Cross-Domain Generalization** (Arxiv 2020) RSC 
5. **UNDERSTANDING THE FAILURE MODES OF OUT-OFDISTRIBUTION GENERALIZATION** (ICLR 2021) 刻画了ERM OOD泛化失败的两种情况：geometric skew和statistical skew。直观理解：geometric skew是由于max-margin classifier由于major part的存在导致为了维持最大margin，不得不使用spurious feature；statistical skew纯粹是由于分析$\frac{w_{\text{sp}(t)}}{w_{\text{inv}(t)}}$的收敛，发现该比值的lower bound会随着spurious correlation p的增加而单调上升。 [[paper]](http://arxiv.org/abs/2010.15775)







## Test-time adaptation for Generalization

### 2024

1. **Context is Environment** (ICLR 2024) [[paper]](https://openreview.net/forum?id=8VPWfqtQMX) 提出以一种ICL的范式来帮助domain generalization， 本质上是一种test-time adaptation

### 2023

1. **Align Your Prompts: Test-Time Prompting with Distribution Alignment for Zero-Shot Generalization** [[paper]](https://arxiv.org/pdf/2311.01459.pdf)
2. 




## Domain Generalization/Adaptation in Down-stream CV/NLP Tasks

### 2023

1. **CLIP the Gap: A Domain Generalization Approach for Object Detection** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Vidit_CLIP_the_Gap_A_Single_Domain_Generalization_Approach_for_Object_CVPR_2023_paper.html)
2. **Improving Zero-Shot Generalization and Robustness of Multi-Modal Models** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Ge_Improving_Zero-Shot_Generalization_and_Robustness_of_Multi-Modal_Models_CVPR_2023_paper.html)
3. **Towards Domain Generalization for Multi-View 3D Object Detection in Bird-Eye-View** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Wang_Towards_Domain_Generalization_for_Multi-View_3D_Object_Detection_in_Bird-Eye-View_CVPR_2023_paper.html)
4. **Single Domain Generalization for LiDAR Semantic Segmentation** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Kim_Single_Domain_Generalization_for_LiDAR_Semantic_Segmentation_CVPR_2023_paper.html)
5. **Modality-Agnostic Debiasing for Single Domain Generalization** (CVPR 2023) [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Qu_Modality-Agnostic_Debiasing_for_Single_Domain_Generalization_CVPR_2023_paper.html)
6. **Improving Zero-Shot Generalization for CLIP with Synthesized Prompts** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Wang_Improving_Zero-Shot_Generalization_for_CLIP_with_Synthesized_Prompts_ICCV_2023_paper.html)
7. **Global Adaptation meets Local Generalization: Unsupervised Domain Adaptation for 3D Human Pose Estimation** (ICCV 2023) [[paper]](https://arxiv.org/abs/2303.16456)
8. **Bayesian Prompt Learning for Image-Language Model Generalization** (ICCV 2023) [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Derakhshani_Bayesian_Prompt_Learning_for_Image-Language_Model_Generalization_ICCV_2023_paper.html) 



## Graph OOD Generalization (graph-level & node-level)

### 2024

1. **Graph Out-of-Distribution Generalization via Causal Intervention** (WWW 2024) [[paper]](http://arxiv.org/abs/2402.11494) node-level。提了一种data driven的方法来推测环境概率分布，并对每个环境的aggregation过程添加一个环境相关的可训练的projector

### 2023

1.  **Towards Understanding Generalization of Graph Neural Networks** (ICLR 2023) 分析了不同GNN架构对泛化误差的影响。特别地，对于GNN最大度数越小，泛化误差越小。 [[paper]](https://openreview.net/forum?id=BhMyLk0YNy)
2.  **Multi-task Self-supervised Graph Neural Networks Enable Stronger Task Generalization** (ICLR 2023) 利用Multiple Gradient Descent来同时优化多个SSL任务的损失，以实现task generalization [[paper]](https://openreview.net/forum?id=1tHAZRqftM)
3.  **Mind the Label Shift of Augmentation-based Graph OOD Generalization (LiSA)** (CVPR 2023) 最小化增强图的分类误差以及增强图与原图（子图和全图）的互信息，同时最大化不同子图之间基于energy function衡量的分布距离，来获得一系列label-invariant的子图（一个子图生成器模拟产生一个环境的数据）。得到不同环境数据后再min loss+var（常规做法） [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Yu_Mind_the_Label_Shift_of_Augmentation-Based_Graph_OOD_Generalization_CVPR_2023_paper.html)
4.  **Joint Learning of Label and Environment Causal Independence for Graph Out-of-Distribution Generalization (LECI)** (Arxiv 2023) **[FIIF&PIIF]** 需要环境标签。优化两个目标：(1)让子图生成器产生分类误差尽量大的spurious subgraph，并对抗地训练一个用spurious graph做预测的分类器 (2)让子图生成器产生无法判别环境的causal graph，并对抗地训练一个环境判别器 [[paper]](https://arxiv.org/abs/2306.01103)
5.  **RETHINKING INVARIANT GRAPH REPRESENTATION LEARNING WITHOUT ENVIRONMENT PARTITIONS (GALA)** (ICLR 2023 Domain Generalization Workshop) 改进版CIGA。讲CIGA的causal contrastive loss改为对齐spurious correlation主导和invariant correlation主导的两部分子集，不同关联主导的数据通过一个额外的ERM模型获得（ERM预测的label不同，就认为是它们属于由不同的关联主导的part） [[paper]](https://openreview.net/forum?id=bjw5jqGtDy)
6.  **FLOOD: A Flexible Invariant Learning Framework for Out-of-Distribution Generalization on Graphs** (KDD 2023) node-level OOD任务。通过已有的图augmentation方法产生一系列增强环境，再使用如VREx的不变学习目标；同时加一个Bootstrapped Representation Learning目标。 [[paper]](https://dl.acm.org/doi/10.1145/3580305.3599355)
7.  **Causality and Independence Enhancement for Biased Node Classification** (CIKM 2023) 把causal feature看成do(c)，然后假设s是c的backdoor，基于此模型来用一个经验性方法建模p(Y|C,S)从而学出causal feature/spurious feature（通过在node feature上加2个MLP实现）。感觉causal graph的假设太强。并且只能解决concept shift。 [[paper]](https://dl.acm.org/doi/10.1145/3583780.3614804)
8.  **Learning Invariant Representations of Graph Neural Networks via Cluster Generalization** (NeurIPS 2023) [[paper]](https://proceedings.neurips.cc/paper_files/paper/2023/file/8ed2293e714b7692b63117e330e551e8-Paper-Conference.pdf) 解决结构shift，semi-supervised setting。通过聚类node feature获得环境，然后利用聚类信息外插node feature做数据增强。
9.  **Stable Prediction on Graphs with Agnostic Distribution Shift** (KDD 2023) node-level，对齐不同环境的aggregation weight（要求数据集是不同环境的图长得一样）和不同环境的loss（VREx）。[[paper]](https://arxiv.org/abs/2110.03865) 
10.  **Individual and Structural Graph Information Bottlenecks for Out-of-Distribution Generalization** (TKDE 2023) 最小化input graph和中间层representation之间的互信息以消除虚假特征，最大化representation和label之间的互信息来学习不变特征。[[paper]](http://arxiv.org/abs/2306.15902)
11.  

###  2022

1. **Learning Substructure Invariance for Out-of-Distribution Molecular Representations** (NeurIPS 2022 Spotlight) 无环境标签的图分类，能拿到多个图（分子）。先推导出了一个ELBO以训练出一个环境判别器，优化目标为最大化图表示 $z$ 与标签 $y$之间的互信息、最小化$I(y;e|z)$。[[paper]](https://openreview.net/forum?id=2nWUNTnFijm) [[notes]](./all_notes/molecular.pdf)
2. **Learning Causally Invariant Representations for Out-of-Distribution Generalization on Graphs (CIGA)**  (NeurIPS 2022) **[FIIF&PIIF]** 首先通过g来预测causal子图，其补图就是spurious subgraph。然后通过推导说明了causal feature与环境无关的条件可以转化为最大化同一类别的feature的互信息（通过一个supervised contrastive loss实现）。在这个约束下，再最大化 $G_s$  $G_c$  分别与y的互信息（$G_s$ 是 $G_c$  的补图，最大化Gs和y的互信息是为了“将Gs从Gc中分离出去”），同时要保证Gs与y的互信息比Gc和y的互信息要小（互信息的优化通过优化loss实现，loss越小互信息越大）[[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/file/8b21a7ea42cbcd1c29a7a88c444cce45-Supplemental-Conference.pdf) [[notes]](./all_notes/nips22_causal.md)
3. **Discovering Invariant Rationales for Graph Neural Networks (DIR)**  (ICLR 2022)  **[FIIF]** 将点之间特征内积最大的子图作为causal子图，同时手动地改变causal子图和spurious子图的组合，模拟多个s的环境。优化目标为优化所有s下的loss以及其方差。其他补充：①加了一个prediction内积项以对spurious correlation进行正则 ②专门学一个用s来预测y的分支，有点类似Learning Causally Invariant Representations for Out-of-Distribution Generalization on Graphs (NIPS 22)的做法，是为了更好地解耦c和s。思考：causal子图的寻找方法缺乏依据 [[paper]](https://arxiv.org/abs/2201.12872) 
4. **Handling Distribution Shifts on Graphs: An Invariance Perspective (EERM)** (ICLR 2022) node-level分类任务。先通过一个AT method ，进行删/增边和最大化每个子图的方差来获得k个子图（来模拟k个环境），然后再min每个子图（环境）上loss的variance和分类loss。[[paper]](https://arxiv.org/abs/2202.02466) [[notes]](./all_notes/EERM.md)
5. **Shift-Robust Node Classification via Graph Adversarial Clustering** (Arxiv 2022) node-level分类任务，open-set DA场景，一个图里有labeled source data和unlabeled target data。该框架可解决new class和new distribution的问题。先在unlabel target domain聚类，然后把聚类模型拿到label source上对齐label。同时为了进一步优化聚类能力，还最小化了source聚类结果和真实label之间的KL散度。[[paper]](https://arxiv.org/pdf/2203.15802.pdf)
6. **GOOD: A Graph Out-of-Distribution Benchmark** (NeurIPS 2022) graph OOD benchmark [[paper]](http://arxiv.org/abs/2206.08452)
7. **Learning Invariant Graph Representations for Out-of-Distribution Generalization** (GIL) graph-level OOD任务。先划分sprious/causal子图，然后拿着spurious子图聚类获得环境。之后用一个VREx目标。 [[paper]](https://openreview.net/forum?id=acKK8MQe2xc)

### 2021

1. **DISTRIBUTIONALLY ROBUST SEMI-SUPERVISED LEARNING OVER GRAPHS** (NeurIPS 2021) 对抗地优化DRO的surrogate loss，来获得对wasserstein距离意义下distributionally robust的模型。[[paper]](http://arxiv.org/abs/2110.10582)
1. **Shift-Robust GNNs: Overcoming the Limitations of Localized Graph Training Data** (NeurIPS 2021) 一种解决node-level OOD 分类的网络。[[paper]](https://proceedings.neurips.cc/paper/2021/file/eb55e369affa90f77dd7dc9e2cd33b16-Paper.pdf)
1. **Mixup for Node and Graph Classification** (WWW 2021) 将表示mixup，并将对应的label mixup起来，就能提升泛化。文章提出了node-level和graph-level分别的mixup方案，后续文章验证了node-level的方案要比graph的表现好 [[paper]](https://dl.acm.org/doi/10.1145/3442381.3449796)

### 2020

1. **DROPEDGE: TOWARDS DEEP GRAPH CONVOLUTIONAL NETWORKS ON NODE CLASSIFICATION** (ICLR)



## Domain Adaptation

### 2023

1. **Diffusion-based Target Sampler for Unsupervised Domain Adaptation** 用diffusion model生成和目标域特征一致的样本来实现DA (Arxiv 2023) [[paper]](https://arxiv.org/pdf/2303.12724.pdf)

### 2019

1. **On Learning Invariant Representations for Domain Adaptation** (ICML2019) 经典之作，指出了学习不变表示的做法的问题：在源域和目标域的label的边缘分布 $p(Y)$ 的差距较大时，对齐源域和目标域不变表示+最小化源域误差的做法会导致目标域误差变大 [[paper]](http://proceedings.mlr.press/v97/zhao19a/zhao19a.pdf) [[notes]](./all_notes/On-Learning-Invariant-Representations-forDA.md)
2. **Support and Invertibility in Domain-Invariant Representations** (PMLR2019) 也claim只对齐表示的边缘分布 $p_s(Z) \approx p_t(Z)$ 对于DA是不够的 [[paper]](http://proceedings.mlr.press/v89/johansson19a.html)







## LLMs

### 2024

1. **Model Editing with Canonical Examples** [[paper]](http://arxiv.org/abs/2402.06155) 提出了一个新任务：让模型学习几个特定的文本例子，以实现某些纠正，同时还不能让模型改变很多。
1. **Evaluating Large Language Models at Evaluating Instruction Following** [[paper]](https://openreview.net/forum?id=tr0KidwPLc) (ICLR 2024) 
1. **Not all Layers of LLMs are Necessary during Inference** (Arxiv April 2024) 训练一个对LLM中间层feature的分类器判断是否应该早停来获取早停层数，来加速LLM推理。还发现中间层预测的top prob和top prob-second top prob在各个任务上都呈现出随着层数加深而增加并逐渐稳定的趋势（但在不同任务上层数不一样）。[[paper]](http://arxiv.org/abs/2403.02181)
1. **Demonstrating Mutual Reinforcement Effect through Information Flow** (Arxiv March 2024) [[paper]](https://arxiv.org/pdf/2403.02902) 研究了同时进行word分类和text分类的MRE（Mutual Reinforcement Effect）任务，也观察到了anchor那篇中的三种attention activation随layer的分布趋势。
1. **A Theoretical Understanding of Self-Correction through In-context Alignment** (Arxiv May 2024) [[paper]](http://arxiv.org/abs/2405.18634) 理论分析transformer中的各个模块在self-correction中发挥的作用
1. **Mechanics of Next Token Prediction with Self-Attention** (AISTATS 2024) [[paper]](https://proceedings.mlr.press/v238/li24f.html) 构造了一个graph来描述next token prediction任务，在简化setting下理论分析出last token更倾向于给更经常作为label的token分配更高的attention。
1. **The pitfalls of next-token prediction** (Arxiv April 2024) [[paper]](http://arxiv.org/abs/2403.06963) 指出了自回归模型的缺陷：错误滚雪球效应和在一个单一token路径上只能学出一个类似induction head的shortcut模型
1. 

### 2023

1. **Instruction-following Evaluation through Verbalizer Manipulation** (Arxiv July 2023) [[paper]](http://arxiv.org/abs/2307.10558) 发现LLM遵循flipped-label instructions的能力很差，说明ICL可能只是直接利用了预训练语料的知识，而不是学习了context。即使是强如GPT-4的模型也不能很好地遵循flipped-label instructions。
2. **Reasoning or Reciting? Exploring the Capabilities and Limitations of Language Models Through Counterfactual Tasks** (Arxiv Aug 2023) [[paper]](http://arxiv.org/abs/2307.02477) 一些主要发现：①模型在counterfactual的setting中性能会变差，且setting和常见的、符合事实的setting相差越远，性能越差，说明了模型可能的记忆现象。②在算术任务上，ICL能提升counterfactual（不同进制的计算）性能，但和default setting的差距难以抹平。
3. **Can the Inference Logic of Large Language Models be Disentangled into Symbolic Concepts?** (Arxiv Apr 2023) [[paper]](https://arxiv.org/abs/2304.01083) 提出了一种empirical的指标来衡量输入句子里的某些词和词组对某一特定输出的决定程度。
4. **Contrastive Chain-of-Thought Prompting** (Arxiv Nov 2023) [[paper]](http://arxiv.org/abs/2311.09277) 使用对比CoT，即一个正确CoT搭配一个错误CoT能相比常规的CoT带来提升.

### 2022

1. **Same Pre-training Loss, Better Downstream: Implicit Bias Matters for Language Models** [[paper]](https://arxiv.org/pdf/2210.14199.pdf)

2. 

### 2021

**LORA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS** 将对模型权重矩阵的更新限制为低秩矩阵乘积$BA$的形式，极大减少了pre-trained model迁移到新任务的代价（不用fine-tune所有参数） [[paper]](https://arxiv.org/abs/2106.09685)

### 2019

1. **Are Sixteen Heads Really Better than One?** (NeurIPS 2019) [[paper]](https://proceedings.neurips.cc/paper_files/paper/2019/hash/2c601ad9d2ff9bc8b282670cdd54f69f-Abstract.html) 在某些层上，只用一个head性能也能保持不变。同时提出了使用attention梯度来衡量head的重要性，提出了剪枝策略。



## LVMs

### 2024

1. **VisionLLaMA: A Unified LLaMA Interface for Vision Tasks** (Arxiv Mar 2024) [[paper]](https://arxiv.org/pdf/2403.00522) Vision LLaMa

### 2022

1. **Data Determines Distributional Robustness in Contrastive Language Image Pre-training (CLIP)** (Arxiv 2022) [[paper]](https://arxiv.org/abs/2205.01397)
2. 





## Prompt Learning

### Prompt learning：
1. **Conditional Prompt Learning for Vision-Language Models** (CoCoOp, CVPR2022) 将图片特征直接加到context token上，获得sample-wise的prompt，以实现instance的generalization。其实就是希望通过引入图像信息来使得prompt描述得更贴切。不过感觉还是有点怪，因为所有class都加上了同样的可学习prefix，为什么能提高预测为正确类的概率？
2. MaPLe: Multi-modal Prompt Learning, CVPR2023 
3. Prompt-aligned Gradient for Prompt Tuning, ICCV2023
4. Compound Text-Guided Prompt Tuning via Image-Adaptive Cues, AAAI2024
5. MmAP : Multi-modal Alignment Prompt for Cross-domain Multi-task Learning, AAAI2024
6. **Improving Zero-Shot Generalization for CLIP with Synthesized Prompts** (ICCV 2023)

### For DA:

1. Domain Adaptation via Prompt Learning, arxiv 2022
2. AD-CLIP: Adapting Domains in Prompt Space Using CLIP, ICCV2023
3. Multi-Prompt Alignment for Multi-Source Unsupervised Domain Adaptation, NIPS2023
4. Prompt-based Distribution Alignment for Unsupervised Domain Adaptation, AAAI2024

### For DG:

1. StyLIP: Multi-Scale Style-Conditioned Prompt Learning for CLIP-based Domain Generalization, arxiv2023





## In-Context Learning

### 2024

1. **Explore Spurious Correlations at the Concept Level in Language Models for Text Classification** (Arxiv Jan 2024) [[paper]](http://arxiv.org/abs/2311.08648) 发现了LLM在文本分类中会依赖的concept-label spurious correlation，提出使用ChatGPT来扩充数据来消除虚假关联。

2. **Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes** (ongoing work) [[paper]](Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes) 发现模型对于demonstration的permutation invariance或许是ICL OOD的关键。提出使用相同的positional encoding来提升ICL OOD性能。

3. **Simple synthetic data reduces sycophancy in large language models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2308.03958) LLMs会迎合提问者的观点而罔顾事实。提出合成一些用户的观点和正确性无关的新prompt，然后在这些数据上fine-tune来解决sycophancy问题。

4. **Understanding In-Context Learning in Transformers and LLMs by Learning to Learn Discrete Functions** (ICLR 2024 Oral) [[paper]](https://arxiv.org/abs/2310.03016) 探究transformer在一系列离散任务上的能力。特别地，发现经过预训练的模型相比随机初始化的模型获得了更强的最近邻、disjunction和conjunction的能力。

5. **Batch-ICL: Effective, Efficient, and Order-Agnostic In-Context Learning**  (Arxiv Jan 2024) 发现使用batch ICL，将N个example设置为N个one-shot inference，再把每个inference得到的token做平均，替换到query sample做aggregation最终再预测能带来提升。一个奇特的发现是做aggregation时从某一层往后做性能会突增，在那之前性能接近零。对此解释是transformer的低层是在学语义信息。

6. **RefuteBench: Evaluating Refuting Instruction-Following for Large Language Models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.13463) 评估模型的改变它们的原始输出并遵循和一开始相违背的指令的能力。主要观察：1)大部分模型都会倾向于遵守它们的预训练知识 2)模型很难根据人类后续的反馈泛化到新的问题 3)所有模型都会逐步忘记人类反馈并落回到它们的内部知识里 4)模型是不是第一时间遵守了人类的反馈，对于后续的行为起到关键作用

7. **Function Vectors in Large Language Models** (ICLR 2024) [[paper]](http://arxiv.org/abs/2310.15213) 发现context prompt的最后一个token的隐层表示encode了这个任务的信息，称为function vector（FV）。将其加到zero-shot的prompt上，发现有显著提升。

8. **A Data Generation Perspective to the Mechanism of In-Context Learning** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.02212) 有关task recognition和task learning的综述

9. **Identifying and Analyzing Task-Encoding Tokens in Large Language Models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2401.11323) 探究了context中的template词（"data:","answer:"）/stopword（标点、连词等无意义词）/content对performance的意义。结果发现template词对ICL性能提升最有用，content反而没什么用；还探究了template词的什么特征使得它有别于context中的其他成分，结果发现template词本身的语义、其重复性、其分隔x和y的格式作用这三者都对ICL性能有显著的作用。

10. **Whispers that Shake Foundations: Analyzing and Mitigating False Premise Hallucinations in Large Language Models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.19103) 发现，问题中的错误前提而导致的回答中的幻觉是由于模型中特定的head的激活所引起的。提出了一种强行消除这些head对于问题中的错误前提对应的token的attention的方法。

11. **In-context Vectors: Making In Context Learning More Effective and Controllable Through Latent Space Steering** (Arxiv Feb 2024) [[paper]](https://arxiv.org/abs/2311.06668) 提出用context的第L层表示构造一个表征任务信息的vector（ICV），然后再加到query时的第L层所有token的表示上。

12. **The mechanistic basis of data dependence and abrupt learning in an in-context classification task** (ICLR 2024 Oral) [[paper]](https://arxiv.org/abs/2312.03002) 有关transformer 的IWL（in-weights learning）和ICL学习过程的实验性分析。在一个两层toy transformer中揭示了induction head学习机制。

13. **Understanding In-context Learning From Repetitions** (ICLR 2024) [[paper]](https://openreview.net/forum?id=bGGYcvw8mp) 揭示了context中重复出现的pattern会导致模型更倾向于输出这个pattern的现象。

14. **In-context Learning Learns Label Relationships but is not Conventional Learning** (ICLR 2024) [[paper]](https://openreview.net/pdf?id=YPIA7bgd5y) 以更大的模型和更长的context重新审视以往的ICL讨论，并得出了以下三个结论：1)ICL会学x-y映射，正确的label是有用的，且模型越大这一效应越明显 2)ICL能学预训练时没见过的新任务 3)即使context很长，ICL也不能彻底覆盖预训练获得的preference 4)LLM更关注更靠近query的example

15. **How do Large Language Models Learn In-Context? Query and Key Matrices of In-Context Heads are Two Towers for Metric Learning** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.02872) 在简单的word classification任务上，首先按照类似Function Vector的做法，提取出对输出正确预测贡献最大的head。然后分析这些head并发现了如下机制：label的V encode了label的特征，label的K encode了demonstration的特征；last token的Q encode了query的特征；last token query和正确label的K的attention score比其他head的显著大；last token Q与在context中出现更多的label/更靠近query的label的K的attention score更大。

16. **Locating Factual Knowledge in Large Language Models: Exploring the Residual Stream and Analyzing Subvalues in Vocabulary Space** (Arxiv Jan 2024) [[paper]](http://arxiv.org/abs/2312.12141) 提出了一种定位transformer中对输出某一label贡献最大的attention或FFN layer（或其subvalue）的方法。

17. **In-Context Learning State Vector with Inner and Momentum Optimization** (Arxiv April 2024) [[paper]](http://arxiv.org/abs/2404.11225) 提了一种新的用vector压缩信息的技术（State Vector SV）：是将前L层的每层的attention输出concat起来。然后提了三种技术（aggregate每一个example的SV、用momentum、分组提取SV再聚合）来进一步优化SV，取得了一些性能提升。

18. **GNNavi: Navigating the Information Flow in Large Language Models by Graph Neural Network** (Arxiv Feb 2024)  [[paper]](http://arxiv.org/abs/2402.11709) 提出将GNN插在LLM的某一层后面，强行使得information flow（token representation就是node representation）是从x->y和y->:连边，然后得到的node representation输给LLM的下一层（每个token的都保留着，因为GNN的输出也是所有node的输出）。最后只在ICL数据集上微调GNN，能够实现和lora媲美的速度和更好的acc。

19. **Decomposing Label Space, Format and Discrimination: Rethinking How LLMs Respond and Solve Tasks via In-Context Learning** (Arxiv April 2024) [[paper]](http://arxiv.org/abs/2404.07546) 将ICL能力分成1)正则化输出的label space、2)正则化输出的label format，和3)提升label space/format分布内的判别能力三个方面。结论：ICL的能力主要来自前两者。同时也在实验上间接证明了ICL会倾向于预测出context和test更像的样本的label。

20. **The Evolution of Statistical Induction Heads: In-Context Learning Markov Chains** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.11004) 在预测Markov序列任务上，揭示了存在一个学习出从简单到复杂function的过程（uniform -> unigram -> bigrams (optimal)）。此外，也验证了类似retrieval（n-gram），即找最相似的context token然后取它后面的token作为预测的机制 

21. **In-Context Language Learning: Architectures and Algorithms** (Arxiv Jan 2024) [[paper]](http://arxiv.org/abs/2401.12973) 构造了一个模拟的language token ICL任务，给了一系列实验证据说明transformer实现了和n-gram类似的retrieval过程

22. **Trusting Your Evidence: Hallucinate Less with Context-aware Decoding** (Arxiv May 2024) [[paper]](http://arxiv.org/abs/2305.14739) 为了增强对context的关注能力，提出在推理时加权以context为条件的预测和不含context的预测：$y=\text{softmax}((1+\alpha) p_\theta(y|c,x)-\alpha p_\theta(y|x))$​ 。背后的理论基础是朴素贝叶斯 [[blog]](https://spaces.ac.cn/archives/9617)

23. **How In-Context Learning Emerges from Training on Unstructured Data: On the Role of Co-Occurrence, Positional Information, and Noise Structures** (Arxiv Jun 2024) [[paper]](http://arxiv.org/abs/2406.00131) 在非ICL格式的数据上训练，探究了“国家-首都”类任务（预训练常见）和输出首字母任务（不常见），发现pattern在训练数据里的重复性和位置信息分别是这两种任务的关键。

24. **Benefits of Transformer: In-Context Learning in Linear Regression Tasks with Unstructured Data** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.00743) 分析多层、PE、multi head等模块对于提升ICL在线性回归任务上性能的作用。

25. **Do pretrained Transformers Learn In-Context by Gradient Descent?** (ICML 2024) [[paper]](http://arxiv.org/abs/2310.08540) 讨论了一下目前ICL工作的不切实际的setting，从一些实验指标上说明了ICL和GD有显著不同。

26. **Rectifying Demonstration Shortcut in In-Context Learning** (NAACL 2024) [[paper]](http://arxiv.org/abs/2403.09488) 发现context单词的字面意思会影响ICL分类的结果（一种shortcut）。提出了一种calibration的策略。

27. **Investigating the Pre-Training Dynamics of In-Context Learning: Task Recognition vs. Task Learning** (Arxiv June 2024) [[paper]](http://arxiv.org/abs/2406.14022) 训练过程中task learning和task recognition存在竞争现象

28. **Transformers Can Perform Distributionally-robust Optimisation through In-context Learning** (ICML 2024 workshop on ICL) [[paper]](https://openreview.net/pdf?id=MOgg2cEms5) ICL有一定的DRO的能力 

29. **How Do In-Context Examples Affect Compositional Generalization?** (ACL 2024) [[paper]](http://arxiv.org/abs/2305.04835) 发现context example对于组合泛化能力影响显著。具体来说，context example和query越像、example越多样、每个样本越简单，泛化能力越好。

30. **What Do Language Models Learn in Context? The Structured Task Hypothesis** (ACL 2024) [[paper]](http://arxiv.org/abs/2406.04216) 通过实验验证了ICL能够对预训练见过的任务进行复合的假设，否定了ICL仅仅能够进行分布内任务的试别以及ICL能够泛化到某些训练时没见过的任务的假设。

31. **What needs to go right for an induction head? A mechanistic study of in-context learning circuits and their formation** (ICML 2024) [[paper]](http://arxiv.org/abs/2404.07129) 识别了transformer在解决ICL的copy-and-paste任务中存在的三种circuit

32. **In-Context Learning of Energy Functions** (ICML 2024 ICL workshop) [[paper]](http://arxiv.org/abs/2406.12785) 提出了将next-token的条件分布建模为能量函数的形式，发现transformer也能在这种形式下展现出ICL能力

    



### 2023

1. **Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?** [[paper]](https://arxiv.org/abs/2202.12837) 做了一系列消融实验来对ICL进行解释。主要结论：即使input和label不是一一对应，只要label的分布合理，那么ICL同样能给出较为正确的答案.
2. **Symbol tuning improves in-context learning in language models** (EMNLP 2023) [[paper]](http://arxiv.org/abs/2305.08298) 将demonstration的label换为无意义的symbol，然后微调，以此强迫模型学习input-label mapping。
3. **In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax** (Arxiv Nov 2023) [[paper] ](In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax) 本文通过构建一些语法任务来测试模型对于句子结构的理解能力，以及OOD泛化性能。总的说来，LLM还是会用到一些spurious correlation。
4. **A Closer Look at In-Context Learning under Distribution Shifts** (Arxiv May 2023) [[paper]](http://arxiv.org/abs/2305.16704) 在一定的分布偏移下，transformer比set-based MLP的性能好；在严重的分布偏移下，两种模型的ICL能力都丧失了。
5. **Few-shot Fine-tuning vs. In-context Learning: A Fair Comparison and Evaluation** (Arxiv May 2023) [[paper]](https://www.lsv.uni-saarland.de/wp-content/uploads/2023/07/Few-shot-Fine-tuning-vs.-In-context-Learning.pdf) 在参数量相当的情况下，ICL的OOD不如FT。30B的ICL跟6.7B的FT性能相当。大部分情况下ICL不如FT。
6. **Instruction-following Evaluation through Verbalizer Manipulation** (Arxiv July 2023) [[paper]](http://arxiv.org/abs/2307.10558) 发现LLM遵循flipped-label instructions的能力很差，说明ICL可能只是直接利用了预训练语料的知识，而不是学习了context。即使是强如GPT-4的模型也不能很好地遵循flipped-label instructions。
7. **Reasoning or Reciting? Exploring the Capabilities and Limitations of Language Models Through Counterfactual Tasks** (Arxiv Aug 2023) [[paper]](http://arxiv.org/abs/2307.02477) 一些主要发现：①模型在counterfactual的setting中性能会变差，且setting和常见的、符合事实的setting相差越远，性能越差，说明了模型可能的记忆现象。②在算术任务上，ICL能提升counterfactual（不同进制的计算）性能，但和default setting的差距难以抹平。
8. **The Learnability of In-Context Learning** (NeurIPS 2023) [[paper]](https://openreview.net/forum?id=f3JNQd7CHM) 证明了当预训练分布包含下游任务的分布的mixuture，ICL能逼近下游任务上的贝叶斯最优分类器。
9. **What In-Context Learning "Learns" In-Context: Disentangling Task Recognition and Task Learning** (Findings of ACL 2023) [[paper]](http://arxiv.org/abs/2305.09731) 分别用随机label（x-y映射关系被破坏）和非自然语言label（x-y映射关系保留）来检验模型的从预训练知识中识别任务和从context中学习input-label映射关系的能力，发现：这两种能力同时存在；任务识别能力基本不随模型规模变化；in-context学习能力会随模型变大而上升。
10. **Larger language models do in-context learning differently** (Arxiv Mar 2023) [[paper]](http://arxiv.org/abs/2303.03846) 和disentanglement TR and TL 那篇差不多，发现了：小模型会倾向于用prior，随着模型增大，覆盖prior而从context学习映射关系的能力会越来越强。
11. **In-Context Learning Creates Task Vectors** (Arxiv Oct 2023) [[paper]](http://arxiv.org/abs/2310.15916) 同样发现context的最后一个token的表示encode了该任务的信息。通过实验发现ICL近似是在实现如下过程：1)从context学出一个映射函数 2)将这个映射函数用到query上来预测。一个重要观察是：说明模型更倾向于使用vector里的信息，而不是原始context
12. **Label Words are Anchors: An Information Flow Perspective for Understanding In-Context Learning** (EMNLP 2023) [[paper]](http://arxiv.org/abs/2305.14160) 浅层网络从text到label聚合信息，深层网络从label到last token聚合信息。
13. **Pretraining Data Mixtures Enable Narrow Model Selection Capabilities in Transformer Models** (Arxiv Nov 2023) [[paper]](http://arxiv.org/abs/2311.00871) 发现ICL在测试和预训练任务不同时性能不好。
14. **Pretraining task diversity and the emergence of non-Bayesian in-context learning for regression** (NeurIPS 2023) [[paper]](https://arxiv.org/abs/2306.15063) 发现预训练学习的任务越多，ICL在新任务上的泛化越强（不同任务：不同线性回归的W）



### 2022

1. **What Can Transformers Learn In-Context? A Case Study of Simple Function Classes** (NeurIPS 2022) [[paper]](https://proceedings.neurips.cc/paper_files/paper/2022/hash/c529dba08a146ea8d6cf715ae8930cbe-Abstract-Conference.html) 实验发现：1)linear function是能通过transformer学到的（性能能逼近最小二乘估计）2)ICL有一定的OOD泛化能力（train -> test, context -> test）3)ICL也能学到更复杂的函数，比如sparse linear functions、ReLU NNs、decision trees。
2. **Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?** (EMNLP 2022) [[paper]](http://arxiv.org/abs/2202.12837) 探究ICL work的因素。
3. **On the Compositional Generalization Gap of In-Context Learning** (Arxiv 2022) [[paper]](http://arxiv.org/abs/2211.08473) 在CFQ等组合泛化任务上测，发现大模型的OOD（query和context不一致）和ID之间的组合泛化能力的gap相比小模型更小。



## ICL Theories

### 2024

1. **How do Transformers perform In-Context Autoregressive Learning?** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.05787) 在限定linear attention、diagonal weight matrix等条件下，对于序列预测任务$s_{T+1}=Ws_T$（文章考虑的$W$是酉矩阵和正交矩阵两种情况），从理论上给出了取到全局最优解时，transformer 参数所应满足的性质。

2. **On Mesa-Optimization in Autoregressively Trained Transformers: Emergence and Capability** (Arxiv May 2024) [[paper]](http://arxiv.org/abs/2405.16845) 理论证明了，不同于直接在ICL目标上进行预训练，经过自回归预训练的one-layer linear attention不能在简单如服从高斯分布的序列上实现ICL。

3. **How Do Nonlinear Transformers Learn and Generalize in In-Context Learning?** (ICML 2024) [[paper]](http://arxiv.org/abs/2402.15607) 在进行ICL预训练的情况下，给出了非线性attention的ID和OOD的泛化保证

4. **Why Larger Language Models Do In-context Learning Differently?** (ICML 2024) [[paper]](http://arxiv.org/abs/2405.19592) 本文对于更大的模型更容易在flipped label任务上失败给了理论解释：大模型更容易受到prompt中noise的影响，而小模型只会关注更重要的feature所以不容易受到noise影响，进而使pretrain feature发挥更大的作用。

5. **Dual Operating Modes of In-Context Learning** (ICML 2024) [[paper]](http://arxiv.org/abs/2402.18819) 理论setting：在混合高斯的线性回归上预训练，分析了给定test context时的后验概率，解释了task recognition和task learning：发现context较短时以task recognition（调整后验的混合高斯的各分量的权重）为主。context变长之后以task learning为主。

6. **In-Context Learning with Transformers: Softmax Attention Adapts to Function Lipschitzness** (Arxiv May 2024) [[paper]](http://arxiv.org/abs/2402.11639) softmax能adaptively学一个attention window来实现将context $y_i$ 进行插值作为预测，将分类任务中见到的retrieval机制拓展到了回归任务上。

### 2023

1. **What learning algorithm is in-context learning? Investigations with linear models** (ICLR 2023) [[paper]](http://arxiv.org/abs/2211.15661) 还没看，理论理解ICL机制的文章，linear regression任务，但它的理论设定是模型要在ICL任务上预训练，与实际的Auto Regressive预训练有较大gap。它的证明思路也是通过网络参数构造解，和A Theoretical Understanding of Self-Correction through In-context Alignment这篇类似。
2. **Transformers as Algorithms: Generalization and Stability in In-context Learning** (ICML 2023) [[paper]](http://arxiv.org/abs/2301.07067) 考虑了context为一系列独立pair和前后样本有关联两种模式，在进行ICL预训练的条件下，给了一个non-linear transformer的excess risk的upper bound
3. **In-Context Convergence of Transformers** (Arxiv Oct 2023) [[paper]](http://arxiv.org/abs/2310.05249) linear regression任务，需要预训练，一层非线性attention，但是做了其他简化使得transforer就是在根据x之间的attention weight来加权组合各个context y作为最终预测。
4. **Trained Transformers Learn Linear Models In-Context** (Arxiv Oct 2023) [[paper]](http://arxiv.org/abs/2306.09927) linear regression任务，需要预训练，一层线性attention。证明了预训练loss收敛到全局最优解时，当训练和测试context足够长时，能学到测试prompt上的正确解W。
5. **What and How Does In-Context Learning Learn? Bayesian Model Averaging, Parameterization, and Generalization** (Arxiv Oct 2023) [[paper]](arXiv:2305.19420) 理论文章，还没看



## Others

### Network Architectures/Neural Network Properties

### 2023

1. **Analyzing Transformers in Embedding Space** [[paper]](http://arxiv.org/abs/2209.02535) Section 2.1 的 transformer architecture讲得比attention is all you need好
1. **The Low-Rank Simplicity Bias in Deep Networks** [[paper]](http://arxiv.org/abs/2103.10427) 神经网络的过参数化（更深的网络层数，比如$W$重参数化为$W=W_1W_2...W_L$）能够使得其学到effective rank更低的表示，从而有利于现实世界任务（比如分类，大部分是想学一个从x到y的低秩映射）上的泛化。

#### 2021

1. **Transformer Feed-Forward Layers Are Key-Value Memories** (EMNLP 2021) [[paper]](http://arxiv.org/abs/2012.14913) transformer的FFN第一层是key，第二层是value
2. 

### Optimization

1. **On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima** (ICLR 2017) large-batch会导致模型更容易进入尖锐的极小值点，而尖锐的极小值点会不利于泛化 [[paper]](https://arxiv.org/abs/1609.04836)

### Adversarial Robustness

#### 2022

1. **The Dimpled Manifold Model of Adversarial Examples in Machine Learning** (arxiv 2022) 从manifold角度研究网络训练的过程以及AT的行为。发现训练DNN大概分为两个过程：①使决策边界由随机快速分布到数据的manifold附近 ②通过在决策边界中产生凹凸，来使决策边界移动到样本的正确一侧。AT会使得决策边界的起伏更大，即向off-manifold方向移动。[[paper]](https://arxiv.org/abs/2106.10151) [[notes]](./all_notes/The-Dimpled-Manifold.pdf)
2. **Understanding Adversarial Robustness Against On-manifold Adversarial Examples** (arxiv 2022) manifold角度研究AT行为，发现了on-manifold对抗样本的存在，提出了在GAN的隐空间做AT、在训练数据的特征向量张成的子空间中做AT两种思路 [[paper]] (https://arxiv.org/abs/2210.00430) [[notes]](./all_notes/Understanding-Adversarial-Robustness-Against.pdf)

### Neural Collapse

#### 2023

1. **Neural Collapse: A Review on Modelling Principles and Generalization** (TMLR 2023)  [[paper]](http://arxiv.org/abs/2206.04041)
1. **How Far Pre-trained Models Are from Neural Collapse on the Target Dataset Informs their Transferability** (ICCV 2023) [[paper]]() 设计了一个empirical的指标来测量目标域模型离NC有多远，来实现以较低开销估计模型在下游目标域的性能（依赖的idea是在目标域上越NC，性能越好）。一个重要的发现是对于有监督/无监督预训练，在源域上within-class variability越大，下游迁移能力越好。

#### 2022

1. **Limitations of Neural Collapse for Understanding Generalization in Deep Learning** (Arxiv 2022) [[paper]](http://arxiv.org/abs/2202.08384) 发现：①训练时和测试时的NC并不一定会同时发生，甚至有可能趋势相反。因此建议对它们分别刻画。②测试时发生NC会导致迁移性更差的特征。（注：这里的”测试时“NC其实并不准确，因为他后面测的实际上是与测试时不同的一个任务：CIFAR10，在”测试时“（也就是预训练）二分类（两个粗类），下游任务10分类。）


### Feature Collapse

1. **Feature Collapse** (Arxiv 2023)

### Causality

1. **Causal Confusion in Imitation Learning** (NeurIPS 2019) 解决imitation learning中的distribution shift问题（解决场景比较像OOD generalization中的correlation shift）。imitation learning：给出observation $X^t$和expert actions $A^t$，希望学出$\pi:X^t\rightarrow A^t$。本文的一个有趣的发现是：观测到太多的（历史）信息会影响泛化性能。[[paper]](https://proceedings.neurips.cc/paper_files/paper/2019/hash/947018640bf36a2bb609d3557a285329-Abstract.html) [[notes&thinking]](./all_notes/Causality.pdf)
2. **Deep Latent-Variable Models** (NeurIPS 2017) 提出了一种VAE来实现存在unobserved confounder时的interventional probability的计算 [[paper]](https://proceedings.neurips.cc/paper/2017/hash/94b5bde6de888ddf9cde6748ad2523d1-Abstract.html) [[notes&thinking]](./all_notes/Causality.pdf)
3. **Fairness in Decision-Making The Causal Explanation Formula** (AAAI 2018) 提出了一种非参数画的counterfactual probability的计算方法，所提出的三种指标DE IE SE分别detect三种discrimination path（从intervention到outcome的path）是否存在，并证明了total variation能够被这三个量表示 [[paper]](https://ojs.aaai.org/index.php/AAAI/article/view/11564) [[notes&thinking]](./all_notes/Causality.pdf)

### Graph Homophily/Heterophily

#### 2023

1. **Characterizing Graph Datasets for Node Classification: Homophily–Heterophily Dichotomy and Beyond** 
   

### Meta-learning

#### 2024

1. **Meta-learning Approaches for Few-Shot Learning: A Survey of Recent Advances** (ACM Computing Surveys 2024) 综述
2. **Advances and Challenges in Meta-Learning: A Technical Review** (TPAMI 2024) 综述

#### 2021

1. **Generalization Bounds For Meta-Learning: An Information-Theoretic Analysis** (NeurIPS 2021) 

#### 2019

1. **Provable Guarantees for Gradient-Based Meta-Learning**

## Slides

主要是从组内的论文分享会讲过的里边挑选出的比较成体系的报告，每个报告一般会包含多篇在某一个小问题上的工作

* 2024-06-12 Recent Advances on ICL Theories [[slides]](./slides/2024.6.11-ICL_Theories.pptx)

* 2024-05-21 Causality Perspective on Domain Generalization [[slides]](./slides/2024.05.21-Causality-OOD.pptx)

* 2024-04-23 Some Empirical Explorations of ICL's Working Mechanism [[slides]](./slides/2024.4.23-ICL_mechanism.pptx) 

* 2024-03-12 In-Context Learning Can Helps the Generalization of Vision Models [[slides]](./slides/2024.3.12-ICL-OOD.pptx)

* 2024-01-10 How Large Vision-Language Models Help OOD Generalization [[slides]](./slides/2024.01.10-OOD-VLM.pptx)

  
