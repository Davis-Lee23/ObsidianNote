张量Tensor，核心有三个属性：rank，shape，type

pytorch：简洁易用、社区活跃 ； 无可视化工具、尚未工业化

mxnet：小众框架

---
最重要的库就是numpy，科学计算库。
pytorch的优点包含：多GPU支持，自定义数据加载器，简化的预处理

torch里面有个autograd，包含了三个属性：data、grad、grad_fn

数据处理：getitem可以返回一条数据或一个样本

dataloader很多参数：
![[Pasted image 20240709151339.png|550]]

torchvision：里面提供了model，dataset，transforms预处理