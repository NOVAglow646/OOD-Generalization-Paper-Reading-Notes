## 对correlation shift和diversity shift的理解

**Part I: 有关correlation shift和diversity shift的定义，和所导出的条件概率不变性思考**

![708c1bc80c7800b5c527eaddd943173](C:\Users\19000\AppData\Local\Temp\WeChat Files\708c1bc80c7800b5c527eaddd943173.jpg)

总结如下：

|                              | $P(X)$                                                       | $P(Y|X)$                    |
| ---------------------------- | ------------------------------------------------------------ | --------------------------- |
| divesity shift (PACS, VLCS): | $P_{tr}(X)\neq P_{te}(X)$                                    | $P_{tr}(X)=P_{te}(X)$       |
| correlation shift (CMNIST):  | support($P_{tr}(X)$)=support($P_{te}(X)$) （若假设$P_{tr}(Y)=P_{te}(Y)$, 且假设风格与label无关，则$P_{tr}(Y|X)=P_{te}(Y|X)$） | $P_{tr}(Y|X)\neq P_{te}(X)$ |



**Part II: spurious feature $Z_2$在实际中如何被估计**

做法：训练一个网络专门用于预测样本来自哪个domain。该网络的特征提取器提取出的特征被视为$Z_2$