#### CROSSMATCH: CROSS-CLASSIFIER CONSISTENCY REGULARIZATION FOR OPEN-SET SINGLE DOMAIN GENERALIZATION（ICLR2022 [6,6,5,5]）

本文试图解决一种新问题：Open-Set Single Domain Generalization (OS-SDG)，即目标域包含源域的标签空间所不包含的标签，在这种情况下对目标域进行分类任务，并把未出现的标签标为unknown。

大致的思路是，先进行data augmentation，通过maximize两个loss，分别生成两类数据，一类是known class的数据$D_k$，一类是包含unknwon class的数据$D_u$。这个阶段对应着maximization过程。之后将$D_k$加入$D_s$(是single training domain的数据)，用$D_s$和$D_u$针对不同的loss去训练普通的多分类器$F$和multi-binary分类器$F_b$，同时要align $F_b$和$F$的预测结果（对应minimize过程）。两个阶段交替迭代。

**stage 1. Minimization**
这一阶段最小化如下的loss：$$L_{min}=L_{ce}+L_{ova}+L^b_{ent}+\alpha L_{ccr} $$
$L_{ce}$是普通的ce loss，用于利用训练$F$.

$L_{ova}(x,y)=-log(p_b^y(t=0|x))-\underset{i\neq y}{min}log(p_b^i(t=1|x)) $，$where~p_b^i(t=0|x)$表示x被分为class i的概率，$p_b^i(t=1|x)$表示x不属于class i的概率，$p_b^i(t=0|x)+p_b^i(t=1|x)=1$.用$D_s$(有标签数据)最小化这个loss希望x尽可能属于其真实标签y，尽可能不属于hardest negative class。直观上理解就是在调整x所属类y的边界让他尽量属于y，并调整离y最近的类的边界让该边界远离x，其实就是在让类别已知的样本的决策边界尽量远离其他类。这可能有助于分开unknown class。

$L^b_{ent}=-\sum_{i=1}^kp_b^i(t=0|x)logp^i_b(t=0|x)+p^i_b(t=1|x)logp^i_b(t=1|x) $，是$F_b$输出的entropy，用$D_u$最小化它，增加分开known class和unknown class的confidence。

$L_{ccr}=\sum_{i=1}^k||p_b^i-p_{b'}^i||^2 $是对$F_b$和$F$预测结果的alignment。希望把$F_b$从$D_u$训练出的关于unknown class的信息传递给$F$.

**stage 2. Maximization**
最大化$L_{max}^k=L_{sdg}(\theta_g,\theta_f;x_s)-\gamma L_{const}(\theta_g;x_k,x_s) $用于生成$D_k$，$L_{sdg}$采取了Generalizing to Unseen Domains via Adversarial Data Augmentation（NIPS 2018）中的对抗样本生成方式，距离度量$L_{const}(\theta_g;x_k,x_s)=||G(x_k)-G(x_s)||^2$用于生成与known class相近的数据。将maximize这个loss得到的样本$D_k$并入$D_s$，供下一轮迭代使用。

最大化$L_{max}^k=L_{unk}(\theta_g,\theta_f;x_s)-\gamma L_{const}(\theta_g;x_k,x_s) $用于生成$D_u$，其中$L_{unk}(\theta_g,\theta_f;x_s)=-log(p_b^y(t=0|x))+\sum_{i\neq y}^klog(p_b^i(t=1|x)) $希望x不属于class y，同时其余所有class都倾向于把x分类为不属于自己，这样就生成了一个属于unknown class的样本。将生成的样本加入$D_u$供下一轮迭代使用。

**Inference**
利用训练好的分类器$F$预测时，将输出熵大于threshold $\mu$的x预测为unknown，否则取正常预测结果。

思考和总结：本文用了交替迭代增强数据的方法，分别生成known class的数据和unknown class的数据，同时在原本的分类器$F$基础上引入一个multi-binary classifier $F_b$，并通过$L_{crr}$建立$F$和$F_b$的联系，以提高对未知类别的识别能力。