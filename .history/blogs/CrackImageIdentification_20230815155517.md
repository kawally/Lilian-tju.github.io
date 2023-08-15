---
layout: page
permalink: /blogs/CrackImageIdentification/index.html
title: CrackImageIdentification
---
## 深度学习在材料性能研究领域的应用————裂纹识别

### 项目名称：基于深度学习算法的质子交换膜裂纹检测

#### 项目简介
在化工、设备等领域常用到各种膜材料，这些膜材料在服役期间可能产生疲劳裂纹。通过疲劳试验机可以直观地观察到材料在承受不同强度的载荷时的裂纹状态，但由于裂纹是不断扩展的，试验机需要一种高效的方法实现对裂纹尖端的
实时追踪。传统的基于机器学习的裂纹识别方法迁移性较差、耗时长；<br>
**基于深度学习的方法因其深层网络结构，无需人工选取图像特征，能够逐层从原始图像中直接学习到不同尺度的特征，使模型具有良好的自适应能力和泛化能力**。<br>
本文描述的工作主要针对质子交换膜材料的裂纹尖端进行识别。由于检测目标较精细，需要较高的定位精度，故选用了目标检测和语义分割两类不同的深度学习算法对裂纹进行检测，并在最后进行了两种方法的优劣对比。
- **目标检测部分选用Faster RCNN 网络，并对 VGG16 和 ResNet101 两种特征提取网络的效果进行对比**；
- **语义分割部分选择 U-Net 网络，并进行参数调整和网络结构优化**。
- 对比实验结果发现，目标检测算法的训练速度更快，效率高；而语义分割算法所依赖的数据量更少，且迁移能力更强。

<div align=center><img src='https://Lilian-tju.github.io/blogs/crack_fig1.jpg'></div>