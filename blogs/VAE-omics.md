---
layout: page
permalink: /blogs/VAE-omics/index.html
title: A Variational Information Bottleneck Approach to Multi-Omics Data Integration
---


## **多组学数据集成的变分信息瓶颈方法(A Variational Information Bottleneck Approach to Multi-Omics Data Integration)**

<div align=left>
<img src="https://Lilian-tju.github.io/blogs/img/VAEfig1.jpg">
</div>


> [2021/艾伦图灵研究所+剑桥大学+UCLA](https://proceedings.mlr.press/v130/lee21a.html)

### Abstract

来自多种组学技术的数据整合在生物医学研究中变得越来越重要。由于组学平台的非一致性和技术限制，对多个组学的这种综合分析（本文称为 views）涉及从各种视图缺失模式的不完整观察中学习。

这是一个挑战，因为

i）需要正确处理观察到的视图内部和之间的复杂交互，以获得最佳预测能力；

ii）需要灵活集成具有各种视图缺失模式的观察结果。

#### 本文工作

- 本文提出了一种用于不完全多视图观测的深度变分信息瓶颈（IB）方法。

- 将IB框架应用于观察到的视图的边缘和联合表示，以关注与目标相关的views内和访谈互动。

- 最重要的是，通过将联合表示建模为边缘表示的产物，我们可以有效地从具有各种视图缺失模式的观察视图中学习。在真实数据集上的实验表明，优于最先进的基准测试。

### introduction 

高通量生物学的技术进步使综合分析包括基因组学、表观基因组学、转录组学、蛋白质组学和代谢组学等数据成为可能，并可以提供对生物系统更全面的理解。

但是，由于来自不同数据源（例如 TCGA1）的实验设计或组成的限制，集成样本通常具有一个或多个完全缺失的组学，具有各种缺失模式。从这种不完整的观察中学习是具有挑战性的。

丢弃缺少组学的样本会大大减少样本量（尤其是在集成许多组学层时），简单的均值插补会严重扭曲数据的边缘分布和联合分布。

在本文中，我们**将多组学数据集成建模为从不完整的多视图观察中学习，其中我们将来自每个组学数据的观察称为view（例如，DNA 拷贝数和 mRNA 表达）。**

然而，直接应用现有的多视图学习方法并不能解决在集成多组学数据时处理缺失视图的关键挑战。这是因为这些方法通常是为全视图观察而设计的，假设所有视图都可用于每个样本 。因此，**我们的目标是开发一个模型，该模型不仅可以学习与目标任务相关的复杂视图内和视图间交互，而且可以灵活地集成观察到的视图，而不管它们的视图缺失模式如何。**

#### 总结
> 提出了一种用于不完全多视图观测的深度变分信息瓶颈（IB）方法称之为深度IMV。
方法由**四个网络组件组成：一组视图特定编码器、一组视图特定预测器、一个专家乘法（PoE）模块和一个多视图预测器。**为了灵活地集成观察到的视图，无论视图丢失模式如何，将联合表示建模为边缘表示上的PoE，这将被多视图预测器进一步利用。
因此，联合表示结合了观察到的视图中的公共信息和补充信息。整个网络按照IB原则进行训练，该原则鼓励边缘和联合表达分别关注与目标相关的视图内和视图间交互。在真实数据集上的实验表明该方法始终从数据集成中获得收益，并且在预测性能度量方面显著优于最先进的基准。

#### Incomplete Multi-View Observations.

多视图观察不完整。大多数现有方法都需要多阶段培训，即根据插补方法构建完整的多视图观察结果，然后培训多视图模型，或者依靠辅助推理步骤生成缺失的视图。
已有方法是以纯无监督的方式训练的。因此，虽然与视图重建相关的信息将在学习表示中很好地捕获，但与目标任务相关的信息可能会丢失。

**我们的工作与 [CPM-Nets](https://papers.nips.cc/paper/2019/file/11b9842e0a271ff252c1903e7132cd68-Paper.pdf)  密切相关。这两种方法都旨在在公共空间中找到表示，以用于预测目标的监督学习。**

与 CPM-Nets 的一个显着区别是我们如何整合不完整的多视图观察。在 CPM-Nets 中，作者直接学习从潜在表示到原始视图的映射，而不使用任何编码器结构。相反，对于所有训练样本，相应的潜在表示被随机初始化，然后迭代更新以最小化重建和分类损失。

**我们推测这种方法有两个局限性：**

- 首先，该方法依赖于重建来寻找测试样本的潜在表示，这使得捕获与任务相关的信息变得困难。

- 其次，随机初始化意味着训练样本的潜在表示之间没有内在关系。因此，必须同时更新所有训练样本，这增加了训练期间的内存负担。

相比之下，在 DeepIMV 中，我们利用**后验分解来灵活地集成观察到的视图**，**并将潜在表示集中到与任务相关的位置，从而有效地捕获用于预测目标的补充信息。**此外，CPM-Nets 仅针对分类任务而设计；因此，对回归任务的扩展可能会失去集群友好表示的优势

#### Information Bottleneck

信息瓶颈（IB）原则是一种信息论方法，它定义了任务相关表示的直观概念，即在**具有简明表示和提供良好预测能力之间的基本权衡**。

使用辅助层将来自每个视图的边缘表示组合成联合表示，在该联合表示上应用IB原理。然而，无法处理训练和测试期间缺失的视图。除此以外，IB原则被扩展到两个视图的无监督设置，以学习两个视图共有的鲁棒表示。这不适用于多组学数据整合，其目标是以监督的方式灵活整合通常包含共同和互补信息的不完整视图（即来自不同组学层的观察结果）

#### Incomplete Multi-View Problem

在多组学数据集成中，缺失视图的存在仍然是一个不可避免且普遍存在的问题。为了解决这个问题，我们首先将这样的综合分析定义为不完整的多视图问题，其中一些视图可能会因任意视图缺失模式而丢失。

解决不完整的多视图问题涉及两个主要挑战：首先，我们想要在公共空间中学习表示，该公共空间利用观察到的视图的边缘和联合方面来进行预测。其次，学习的表示必须灵活地集成在统一框架中具有各种视图缺失模式的不完整观察。

### Method: DeepIMV

为了应对这些挑战，我们提出了一种深度变化信息瓶颈方法，我们称之为DeepIMv4，它由四个网络组件组成，如图1所示：

<div align=center>
<img src="https://Lilian-tju.github.io/blogs/img/VAEfig2.jpg">
</div>


V = 3视图的拟议网络体系结构图示。为了说明起见，我们假设当前样本缺少第二个视图，即x2 = 没有。在这里，虚线对应于根据各自的分布在潜在表示中绘制样本，绿色和蓝色的线表示边缘表示和相应的预测。

> [**<font color='red'> 对于文献理解点击此处：组会report：VAE-omics</font>**](https://Lilian-tju.github.io/blogs/reports/20220823-多组学数据集成的变分信息瓶颈方法.pdf) 