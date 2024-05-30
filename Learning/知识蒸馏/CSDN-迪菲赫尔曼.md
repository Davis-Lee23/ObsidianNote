[一文搞懂【知识蒸馏】【Knowledge Distillation】算法原理_知识蒸馏算法-CSDN博客](https://blog.csdn.net/weixin_43694096/article/details/127505946)

——了解一下知识蒸馏

### 1、什么是知识蒸馏

知识蒸馏可以理解为把一个**大的臃肿的教师模型**的知识萃取出来浓缩蒸馏给一个**小的轻量级的学生模型**，相当于知识的迁移。这样学生模型可以去做一些轻量化网络做的事情。

### 2、轻量化网络的方式有哪些？

感觉1和3可以看一下
1. 压缩已训练好的模型：知识蒸馏、权值量化、权重剪枝、通道剪枝、注意力迁移
2. 直接训练轻量化网络：SqueezeNet、MobileNetv1v2v3、MnasNet、SHhffleNet、Xception、EfficientNet、EfficieentDet
3. 加速卷积运算：im2col+GEMM、Wiongrad、低秩分解
4. 硬件部署：TensorRT、Jetson、TensorFlow-Slim、openvino、FPGA集成电路

### 3、为什么要进行知识蒸馏？

DL在很多领域取得了很好的效果，但是大多数模型计算上过于昂贵，无法在移动端或者嵌入式设备上运行，因此需要对模型进行压缩，这样终端设备就可以部署这些小模型了。

#### 3.1 提升模型精度



### 4、知识蒸馏的理论依据



