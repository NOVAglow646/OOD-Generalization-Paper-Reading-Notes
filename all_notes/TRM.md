#### Learning Representations that Support Robust Transfer of Predictors

思路是把模型在不同domain的泛化能力直接作为objective优化。提出了TRM(Transfer Risk Minimization)。所谓的Transfer Risk，也就是将一个环境的最优分类器在其他环境上的泛化能力进行平均得到的risk。可以用下面的loss来衡量：

$w$是分类器,$\Phi$是特征表示，$\Omega$是环境集合，P、Q是对应环境的empirical distribution。对于给定的$\Phi$，TRM所提出的泛化能力衡量标准如下：
$$R(\Phi;\Omega)=\underset{Q\in \Omega}{\sum}(\underset{P \in Conv(\Omega/Q)}{sup} E_P[l(w(Q;\Phi)\circ \Phi)]) $$其中$w(Q;\Phi)=\underset{w}{arg~min}E_Q[l(w\circ\Phi)]$是$\Phi$在Q上的最优分类器，$Conv(\Omega/Q)$是$\Omega$中除去Q的环境的凸包。

##### 优化：

1、内层优化：需要对抗地找到让$w(Q;\Phi)$性能最差的环境P，要搜索凸包$Conv(\Omega/Q)=\{\underset{P_i \in \Omega/Q}{\sum} \alpha_i(Q)P_i|\alpha_i(Q)\geq 0, ||\alpha(Q)||_1=1 \}$中的元素P，于是需要对$\alpha$进行优化。做法是对$\alpha_i$进行Exponential Gradient Ascent。

2、外层优化：需要通过优化$\Phi$来让$w(Q;\Phi)$在$P(Q)$上的loss最低。将$w(Q;\Phi)$在$P(Q)$上的loss$E_{P(Q)}[l(w(Q;\Phi)\circ \Phi)]$记为$L_P(Q)$。对$\Phi$的全梯度$\frac{d L_P(Q)}{d \Phi}$可以写成两项，
$$\frac{\partial L_P(Q)}{\partial \Phi}+
(\frac{\partial L_P(Q)}{\partial w(Q;\Phi)})^T
\frac{dw(Q;\Phi)}{d\Phi}\tag{1}$$
第二项（implicit gradient）可以改写为$$-\frac{\partial(sg(
    (\frac{\partial L_P(Q)}{w(Q)})^TH^{-1}_{w(Q)}
)^T(\frac{\partial E_Q[l(w(Q))]}{\partial w(Q)}))}{\partial \Phi}\tag{2})$$其中$H_{W(Q)}=\frac{\partial^2 E_Q[l(w(Q))]}{\partial w(Q)^2}$，$sg$为stop_gradient操作，即让括号内的函数不对$\Phi$求导。
***证明**：注意到，$w(Q;\Phi)$是由方程$\frac{\partial E_Q[l(w(Q)\circ \Phi)]}{\partial w(Q)}=0$确定的隐函数。由隐函数存在定理，$$\frac{d w(Q)}{d \Phi}=-\frac{\frac{\partial E_Q[l(w(Q)\circ \Phi)}{\partial \Phi}}{\frac{\partial E_Q[l(w(Q)\circ \Phi)}{\partial w(Q)}}
=-H^{-1}_{w(Q)}\frac{\partial^2 E_Q[l(w(Q)\circ \Phi)]}{\partial w(Q) \partial \Phi}$$代入implicit gradient，即可得证。*

将(2)代入(1)并对$\Phi$积分，得到外层优化的损失函数:$$E_P[l(w(Q)\circ \Phi)]-(sg(
    (\frac{\partial L_P(Q)}{w(Q)})^TH^{-1}_{w(Q)}
)^T(\frac{\partial E_Q[l(w(Q))]}{\partial w(Q)}))+C\tag{3}$$

第一项保证环境Q上的最优分类器w(Q)在P上的性能；第二项是环境Q和w(Q)的最差性能环境P(Q)对w(Q)梯度的alignment。

第二项相比于Fish直接align不同domain的梯度：$$\mathcal{L}_{idgm}=\mathcal{L}_{erm}(\mathcal{D}_{tr};\theta)-\gamma\frac{2}{S(S-1)}\sum^{i\neq j}_{i,j\in S}G_iG_j $$
其中，$G_i=E_{\mathcal{D}_i}\frac{\partial{l(x,y;\theta)}}{\partial \theta}$，区别在于TRM加入了Hessian Inverse这一项，而且是对最优分类器的导数。

对Hessian Inverse的近似：对$H^{-1}$泰勒展开到前j项：$$H_{j}^{-1}=\sum_{i=0}^j(I-H)^i$$可以用递推式计算$H^{-1}_{j}$：$$H^{-1}_{j}=I+(I-H)H^{-1}_{j-1}$$递推式的矩阵乘法用参考Fast Exact Multiplication by the Hessian，可以在线性时间内完成。

**算法：**
将（3）的损失函数加上ERM的loss $E_Q[l(w_{all}\circ \Phi)]$用于更新总的分类器$w_{all}$，在给定$P(Q)$的条件下得到环境$Q$的loss：
$$R(\Phi,w_{all};Q)=E_Q[l(w_{all}\circ \Phi)]+E_P[l(w(Q)\circ \Phi)]-\lambda\frac{\partial(sg(
    (\frac{\partial L_P(Q)}{w(Q)})^TH^{-1}_{w(Q)}
)^T(\frac{\partial E_Q[l(w(Q))]}{\partial w(Q)}))}{\partial \Phi}\tag{3}$$

