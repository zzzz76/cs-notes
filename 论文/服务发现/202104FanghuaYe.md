# 202104 Fanghua Ye

Fanghua Ye, Zhiwei Lin, Chuan Chen, Zibin Zheng, and Hong Huang. 2021. Outlier-Resilient Web Service QoS Prediction. In Proceedings of the Web Conference 2021 (WWW '21)

摘要：尽管已经提出了各种Qos的预测方法，但很少有方法考虑到离群值，这可能会极大地降低预测性能。本文提出了一种抗异常值的Qos预测方法，该方法利用Cauchy loss来衡量观察到的Qos值和预测值之间的差异，通过考虑时间信息来提供时间感知的Qos预测结果。实验结果表明，我们的方法能够取得比最先进的基线方法更好的性能。

keywords: Web service, Qos prediction, outlier resilience, Cauchy loss



## 研究背景

由于服务调用的上下文存在差异，不同用户对于相同Web服务的Qos评价是不一样的。因此，为不同用户提供个性化的Qos评价是必要的。然而，由于高昂的时间成本和巨大的资源开销，用户无法调用所有的Web服务来获得相应的Qos值。因此，基于已有的观测值预测未知的Qos评价是至关重要的。



## 主要问题

矩阵因子分解(MF)是最为常用的Qos预测方法，但是现有的基于mf的Qos预测方法大多直接利用L2-norm来度量QoS观测值与预测值的差值。在实际情况下，L2-norm对于异常值是十分敏感的，无法达到令人满意的性能表现。



## 创新点

基于矩阵因子分解框架，本文提出了一种具有异常值弹性的健壮Web服务QoS预测方法。该方法通过柯西损失来度量观测值和预测值之间的差异，对异常值具有较强的鲁棒性。接着，基于张量分解框架，本文对提出的方法进一步扩展，通过考虑时间信息来提供时间感知的QoS预测结果。最后，本文在静态和动态数据集上进行了广泛的实验，结果表明 ，本文的方法可以达到比最先进的基线方法更好的性能。



Can we achieve robust Qos prediction without detecting outliers explicitly

我们能否在不显式检测离群值的情况下实现鲁棒的Qos预测

## 评价

以后的所有L1-norm和L2-norm相关的论文均可以用这种方法增强鲁棒性

看一下别人是如何引用的

## 相关知识

Matrix Factorization

