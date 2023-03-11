## 对correlation shift和diversity shift的理解

**Part I: 有关correlation shift和diversity shift的定义，和所导出的条件概率不变性思考**

![708c1bc80c7800b5c527eaddd943173](C:\Users\19000\AppData\Local\Temp\WeChat Files\708c1bc80c7800b5c527eaddd943173.jpg)

总结如下：设$Z$代表虚假特征。

|                              | $P(X)$                                    | $P(Y|X)$                                                     |
| ---------------------------- | ----------------------------------------- | ------------------------------------------------------------ |
| divesity shift (PACS, VLCS): | $P_{tr}(X)\neq P_{te}(X)$                 | $P_{tr}(Y|X)$不一定$=P_{te}(Y|X)$，因为given $z\in Z$，$P_{tr}(Y|Z)$不一定$=P_{te}(Y|Z)$ |
| correlation shift (CMNIST):  | support($P_{tr}(X)$)=support($P_{te}(X)$) | $P_{tr}(Y|X)\neq P_{te}(Y|X)$，因为$P_{tr}(Y|Z)\neq P_{te}(Y|Z)$ |



**Part II: spurious feature $Z_2$在实际中如何被估计**

做法：训练一个网络专门用于预测样本来自哪个domain。该网络的特征提取器提取出的特征被视为$Z_2$