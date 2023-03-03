本质是就是给style transfer for DG 的方法套上了一个理论框架，看起来很cool。

实现细节：

1. 对于CMNIST，直接变换背景颜色。对于PACS，用CycleGAN学从一个domain到另一个domain的风格迁移。

一些疑惑：

1. CIT如果是恒等变换呢？原文好像并没限制不行（证明细节待check），这显然是个trivial的solution。