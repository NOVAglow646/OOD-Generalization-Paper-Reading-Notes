本质是就是给style transfer for DG 的方法套上了一个理论框架，看起来很cool。

实现细节：

1. 对于CMNIST，直接变换背景颜色。对于PACS，用CycleGAN学从一个domain到另一个domain的风格迁移。

疑问：

1. CIT可不可以是恒等变换？【答】不行。虽然定义上没有限制它不能是恒等变换，但Theo. 2 要求变换得max loss。

个人认为文章可以改进的地方：

1. CIT没有给出泛化误差bound与tranformation的强度、OOD数据集难度的关系，而且对于CIT的寻找，仍然采用的是启发式方法。换言之，理论虽然保证了CIT可以泛化到OOD数据，但是实际操作中对于理想CIT的寻找仍然没有理论保证。
1. CIT的做法仍然需要GAN这种复杂的数据增广方式。但是根据On the Strong Correlation Between Model Invariance and Generalization的结果，对于简单的灰度变换的output consistency就能体现较好的OOD泛化效果。