## Preface

本仓库记录关于OOD Generalization/Domain Generlization/Domain Adaptation/Causality等topic的文章。看过的文章会至少用一句话概括内容，有些还会有notes。只有标题的就是还没看过的，只是先存档到这里。

###  🔥 Updates

- 2024-02-21 接下来将会主要关注LLM/Multi-modal LLMs的generalization。

## Directory

* [OOD Generalization of (Multimodal) LLMs](ood-generalization-of-multimodal-LLMs)



## OOD Generalization of (Multimodal) LLMs 

### 2024

1. **On the Out-Of-Distribution Generalization of Multimodal Large Language Models** (Arxiv Feb 2024) [[paper]](http://arxiv.org/abs/2402.06599) 对MLLM进行了一系列OOD性能的验证，发现：①在经典OOD数据集上MLLM能超过以前的sota ②在医学、分子数据集上效果不好 ③ICL能提升MLLM OOD性能，即使ICL example的分布和测试域不同。

### 2023

1. **In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax** (Arxiv Nov 2023) [[paper] ](In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax) 本文通过构建一些语法任务来测试模型对于句子结构的理解能力，以及OOD泛化性能。总的说来，LLM还是会用到一些spurious correlation。
2. **On the Robustness of ChatGPT: An Adversarial and Out-of-distribution Perspective** (ICLR 2024 workshop highlighted) [[paper]](http://arxiv.org/abs/2302.12095)
3. **Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes** (ongoing work) [[paper]](Positional Information Matters for Invariant In-Context Learning: A Case Study of Simple Function Classes) 发现模型对于demonstration的permutation invariance或许是ICL OOD的关键。提出使用相同的positional encoding来提升ICL OOD性能。
4. **A Closer Look at In-Context Learning under Distribution Shifts** (Arxiv May 2023) [[paper]](http://arxiv.org/abs/2305.16704) 



## OOD Generalization using Large Vision-language models

### 2023

1. **PromptStyler: Prompt-driven Style Generation for Source-free Domain Generalization** (ICCV 2023) 先训练K个用于生成style word embedding（多样性+保持语义），然后把它们和N个类别词embedding结合，喂给CLIPtext生成K*N个style-content的text feature。之后拿这些feature训练一个linear classifier。推断时把这个classifier接到CLIP image encoder上（**因为在joint image-text空间中，存在cross-modal transferability phenomenon**）。 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Cho_PromptStyler_Prompt-driven_Style_Generation_for_Source-free_Domain_Generalization_ICCV_2023_paper.html)
3. **A Sentence Speaks a Thousand Images: Domain Generalization through Distilling CLIP with Language Guidance** (ICCV 2023) 将CLIP的language encoder输出的embedding作为"generic text representation"，然后让student（visual model）的表示去对齐teacher（CLIP）的text representation。同时对齐student和teacher的预测logit。【insight 1】recent work发现基于环境划分的方法不那么work，因为真实世界的环境划分不明确 【insight 2】从优化难度角度说明anchor sample在对齐时的作用 [[paper]](https://openaccess.thecvf.com/content/ICCV2023/html/Huang_A_Sentence_Speaks_a_Thousand_Images_Domain_Generalization_through_Distilling_ICCV_2023_paper.html) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
4. **Distilling Out-of-Distribution Robustness from Vision-Language Foundation Models** (NIPS 2023) 在用In domain数据生成的Discrete adversarial eaxmple上拿一个CLIP做蒸馏提升就能超过普通的Knowledge Distillation和DAT [[paper]](https://arxiv.org/pdf/2311.01441.pdf) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
5. **Distilling from Vision-Language Models for Improved OOD Generalization in Vision Tasks** (Arxiv Oct 2023) 对齐student model经过一个projector后的表示和CLIP的text/image encoder的输出 [[paper]](https://arxiv.org/abs/2310.08255) [[slides]](/all_notes/2023.12.29-OOD-LM.pptx)
6. **Context-Aware Robust Fine-Tuning** (IJCV Dec 2023) 发现微调会破坏CLIP原有的对于context（虚假特征）zero-shot的分类能力。提出在微调的时候对齐预训练的CLIP预测的context的概率分布和正在被微调的模型预测的context的概率分布 [[paper]](https://link.springer.com/article/10.1007/s11263-023-01951-2) 
7. **ArGue: Attribute-Guided Prompt Tuning for Vision-Language Models **(Arxiv Nov 2023) [[paper]](http://arxiv.org/abs/2311.16494) 本文提出了neg prompt，就是在prompt里加入neg attribute的embedding。在prompt tuning时，希望在使用neg prompt时，预测成任何类别的概率相同，以此来强行消除虚假关联（文中loss式(9)）。使用neg prompt时预测为某一类的概率就是用neg embedding+任何class的embedding和图像表示算相似度。



## New tasks of OOD Generalization

1. **SimMMDG: A Simple and Effective Framework for Multi-modal Domain Generalization** (NeurIPS 2023) 多模态DG，将各模态的特征分为不同模态share的部分以及各个模态specific的部分，拉近同一class的不同模态shared部分的特征，推远share部分的特征和specific部分的特征。[[paper]](https://openreview.net/forum?id=RiSMijlsLT)



## OOD Generalization (classic)
### 2024
1. **Spurious Feature Diversification Improves Out-of-distribution Generalization** (ICLR 2024 under review) 通过ensemble学更多的spurious feature能“冲淡”它们各自的影响 [[paper]](https://openreview.net/forum?id=d6H4RBi7RH)
1. **Out-Of-Domain Unlabeled Data Improves Generalization** (ICLR 2024 spotlight) [[paper]](https://openreview.net/forum?id=Bo6GpQ3B9a)

### 2023

1. **FREE LUNCH FOR DOMAIN ADVERSARIAL TRAINING: ENVIRONMENT LABEL SMOOTHING** (ICLR2023) 使用简单的domain label smoothing方法来解决DANN训练不稳定的问题，给出了丰富的理论支持 [[paper]](https://arxiv.org/abs/2302.00194)[[论文分享会slides]](./all_notes/2023.2.17domain_adversarial_training_with_ELS.pdf)

2. **Generalizability of Adversarial Robustness Under Distribution Shifts** (AAAI2023) AT有助于Domain Adaptation，但是在Domain Generalization setting下不如正常模型 [[paper]](https://arxiv.org/abs/2209.15042)

3. **WHAT IS MISSING IN IRM TRAINING AND EVALUATION? CHALLENGES AND SOLUTIONS** (ICLR2023) 指出large-batch会使得各IRM方法的结果不准，small-batch的效果足以匹敌基于large-batch的优化算法。指出了CMNIST只测-90%是不完善的。基于IRM-GAME提出了BLOC-IRM，发现显示地引入per-environment stationary正则项能缓解各domain分类器一致的这个约束带来的各环境分类器次优的问题。[[paper]](https://openreview.net/forum?id=MjsDeTcDEy)

4. **Aggregation of Disentanglement: Reconsidering Domain Variations in Domain Generalization** (arxiv2023) 使用contrastive learning来拉近统一domain数据的表示、推远不同domain数据的表示（文章claim自己是首个在contrastive learning for DG中考虑不同domain的），同时提出所谓的domain-specific feature也有助于泛化（但是文章的实现算法貌似没有显式的解耦invariant/variant feature的过程，也没有显示地构建domain-specific feature）[[paper]](https://arxiv.org/abs/2302.02350)

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

### 2023

1.  **Towards Understanding Generalization of Graph Neural Networks** (ICLR 2023) 分析了不同GNN架构对泛化误差的影响。特别地，对于GNN最大度数越小，泛化误差越小。 [[paper]](https://openreview.net/forum?id=BhMyLk0YNy)
2.  **Multi-task Self-supervised Graph Neural Networks Enable Stronger Task Generalization** (ICLR 2023) 利用Multiple Gradient Descent来同时优化多个SSL任务的损失，以实现task generalization [[paper]](https://openreview.net/forum?id=1tHAZRqftM)
3.  **Mind the Label Shift of Augmentation-based Graph OOD Generalization (LiSA)** (CVPR 2023) 最小化增强图的分类误差以及增强图与原图（子图和全图）的互信息，同时最大化不同子图之间基于energy function衡量的分布距离，来获得一系列label-invariant的子图（一个子图生成器模拟产生一个环境的数据）。得到不同环境数据后再min loss+var（常规做法） [[paper]](https://openaccess.thecvf.com/content/CVPR2023/html/Yu_Mind_the_Label_Shift_of_Augmentation-Based_Graph_OOD_Generalization_CVPR_2023_paper.html)
4.  **Joint Learning of Label and Environment Causal Independence for Graph Out-of-Distribution Generalization (LECI)** (Arxiv 2023) **[FIIF&PIIF]** 需要环境标签。优化两个目标：(1)让子图生成器产生分类误差尽量大的spurious subgraph，并对抗地训练一个用spurious graph做预测的分类器 (2)让子图生成器产生无法判别环境的causal graph，并对抗地训练一个环境判别器 [[paper]](https://arxiv.org/abs/2306.01103)
5.  **RETHINKING INVARIANT GRAPH REPRESENTATION LEARNING WITHOUT ENVIRONMENT PARTITIONS (GALA)** (ICLR 2023 Domain Generalization Workshop) 改进版CIGA。讲CIGA的causal contrastive loss改为对齐spurious correlation主导和invariant correlation主导的两部分子集，不同关联主导的数据通过一个额外的ERM模型获得（ERM预测的label不同，就认为是它们属于由不同的关联主导的part） [[paper]](https://openreview.net/forum?id=bjw5jqGtDy)
6.  **FLOOD: A Flexible Invariant Learning Framework for Out-of-Distribution Generalization on Graphs** (KDD 2023) node-level OOD任务。通过已有的图augmentation方法产生一系列增强环境，再使用如VREx的不变学习目标；同时加一个Bootstrapped Representation Learning目标。 [[paper]](https://dl.acm.org/doi/10.1145/3580305.3599355)
7.  **Causality and Independence Enhancement for Biased Node Classification** (CIKM 2023) 把causal feature看成do(c)，然后假设s是c的backdoor，基于此模型来用一个经验性方法建模p(Y|C,S)从而学出causal feature/spurious feature（通过在node feature上加2个MLP实现）。感觉causal graph的假设太强。并且只能解决concept shift。 [[paper]](https://dl.acm.org/doi/10.1145/3583780.3614804)

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



## Adversarial Robustness

### 2022

1. **The Dimpled Manifold Model of Adversarial Examples in Machine Learning** (arxiv 2022) 从manifold角度研究网络训练的过程以及AT的行为。发现训练DNN大概分为两个过程：①使决策边界由随机快速分布到数据的manifold附近 ②通过在决策边界中产生凹凸，来使决策边界移动到样本的正确一侧。AT会使得决策边界的起伏更大，即向off-manifold方向移动。[[paper]](https://arxiv.org/abs/2106.10151) [[notes]](./all_notes/The-Dimpled-Manifold.pdf)
2. **Understanding Adversarial Robustness Against On-manifold Adversarial Examples** (arxiv 2022) manifold角度研究AT行为，发现了on-manifold对抗样本的存在，提出了在GAN的隐空间做AT、在训练数据的特征向量张成的子空间中做AT两种思路 [[paper]] (https://arxiv.org/abs/2210.00430) [[notes]](./all_notes/Understanding-Adversarial-Robustness-Against.pdf)



## LLMs/Large Multi-modal models

### 2024

1. **Model Editing with Canonical Examples** [[paper]](http://arxiv.org/abs/2402.06155) 提出了一个新任务：让模型学习几个特定的文本例子，以实现某些纠正，同时还不能让模型改变很多。

### 2022

1. **Same Pre-training Loss, Better Downstream: Implicit Bias Matters for Language Models** [[paper]](https://arxiv.org/pdf/2210.14199.pdf)
2. **Data Determines Distributional Robustness in Contrastive Language Image Pre-training (CLIP)** (Arxiv 2022) [[paper]](https://arxiv.org/abs/2205.01397)



### 2021

**LORA: LOW-RANK ADAPTATION OF LARGE LANGUAGE MODELS** 将对模型权重矩阵的更新限制为低秩矩阵乘积$BA$的形式，极大减少了pre-trained model迁移到新任务的代价（不用fine-tune所有参数） [[paper]](https://arxiv.org/abs/2106.09685)



## Neural Collapse

### 2023

1. **Neural Collapse: A Review on Modelling Principles and Generalization** (TMLR 2023)  [[paper]](http://arxiv.org/abs/2206.04041)

### 2022

1. **Limitations of Neural Collapse for Understanding Generalization in Deep Learning** (Arxiv 2022) [[paper]](http://arxiv.org/abs/2202.08384)



## Feature Collapse

1. **Feature Collapse** (Arxiv 2023)



## Causality

1. **Causal Confusion in Imitation Learning** (NeurIPS 2019) 解决imitation learning中的distribution shift问题（解决场景比较像OOD generalization中的correlation shift）。imitation learning：给出observation $X^t$和expert actions $A^t$，希望学出$\pi:X^t\rightarrow A^t$。本文的一个有趣的发现是：观测到太多的（历史）信息会影响泛化性能。[[paper]](https://proceedings.neurips.cc/paper_files/paper/2019/hash/947018640bf36a2bb609d3557a285329-Abstract.html) [[notes&thinking]](./all_notes/Causality.pdf)
2. **Deep Latent-Variable Models** (NeurIPS 2017) 提出了一种VAE来实现存在unobserved confounder时的interventional probability的计算 [[paper]](https://proceedings.neurips.cc/paper/2017/hash/94b5bde6de888ddf9cde6748ad2523d1-Abstract.html) [[notes&thinking]](./all_notes/Causality.pdf)
3. **Fairness in Decision-Making The Causal Explanation Formula** (AAAI 2018) 提出了一种非参数画的counterfactual probability的计算方法，所提出的三种指标DE IE SE分别detect三种discrimination path（从intervention到outcome的path）是否存在，并证明了total variation能够被这三个量表示 [[paper]](https://ojs.aaai.org/index.php/AAAI/article/view/11564) [[notes&thinking]](./all_notes/Causality.pdf)

## Graph Homophily/Heterophily

### 2023

1. **Characterizing Graph Datasets for Node Classification: Homophily–Heterophily Dichotomy and Beyond** 
2. 



## 其他杂文

1. **On Large-Batch Training for Deep Learning: Generalization Gap and Sharp Minima** (ICLR2017) large-batch会导致模型更容易进入尖锐的极小值点，而尖锐的极小值点会不利于泛化 [[paper]](https://arxiv.org/abs/1609.04836)




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
4. **Batch ICL** 发现使用batch ICL，将N个example设置为N个one-shot inference，再把每个inference得到的token做平均，替换到query sample做aggregation最终再预测能带来提升。一个奇特的发现是做aggregation时从某一层往后做性能会突增，在那之前性能接近零。对此解释是transformer的低层是在学语义信息。



### 2023

1. **Rethinking the Role of Demonstrations: What Makes In-Context Learning Work?** [[paper]](https://arxiv.org/abs/2202.12837) 做了一系列消融实验来对ICL进行解释。主要结论：即使input和label不是一一对应，只要label的分布合理，那么ICL同样能给出较为正确的答案.
2. **Symbol tuning improves in-context learning in language models** (EMNLP 2023) [[paper]](http://arxiv.org/abs/2305.08298) 将demonstration的label换为无意义的symbol，然后微调，以此强迫模型学习input-label mapping。
3. **In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax** (Arxiv Nov 2023) [[paper] ](In-context Learning Generalizes, But Not Always Robustly: The Case of Syntax) 本文通过构建一些语法任务来测试模型对于句子结构的理解能力，以及OOD泛化性能。总的说来，LLM还是会用到一些spurious correlation。
4. 
