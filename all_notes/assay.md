### Take-away message

1. 不同的OOD任务和数据集会导致metric跟OOD acc的关系变化巨大。Appendix A4

2. 不同数据集的OOD难度：（measured by $\text { gap }=\frac{\operatorname{acc}_{\mathrm{ID}}-\mathrm{acc}_{O O D}}{\operatorname{acc}_{\mathrm{ID}}}$）<img src="D:\Documemts\WeChatData\WeChat Files\wxid_9kaq4mokf0dv11\FileStorage\Temp\1677987638677(1).png" alt="1677987638677(1)" style="zoom:50%;" />

   但这个difficulty的定义似乎与文章所给出的指标关系不明显。或许说明用acc的gap来衡量OOD难度不是一个好的指标。

   <img src="C:\Users\19000\AppData\Roaming\Typora\typora-user-images\image-20230305114215390.png" alt="image-20230305114215390" style="zoom:50%;" />

3. 文章没有发现（起码在他测试的指标中）比ID accuracy更好的OOD指标