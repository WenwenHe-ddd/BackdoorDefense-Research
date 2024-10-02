# How To Backdoor Federated Learning

（主体是Backdoor，问题是如何在联邦学习中使用）

 [how to Backdoor Federated learning.pdf](how to Backdoor Federated learning.pdf) 

## 问题

**（Related work中总结）如何在联邦学习背景下实现后门攻击与防御**

\1.    如何实现并防御训练时攻击、测试时攻击？虽然大多数传统方法都实现了，但是并不适用于联邦学习背景。

\2.    如何使用多方计算？使用了多方计算有利于后门攻击的进行，但是实现的方法不适用联邦学习背景。

\3.    如何实现参与者差异隐私？虽然能实现参与者差异隐私，但是还不清楚知识转移是如何进行的

\4.    如何实现收敛？拜占庭分布式学习（数据相同分布），但是不符合联邦学习的背景

## 动机

\1.    以前通常都是采用对训练数据中毒攻击来解决这个问题，但是鲜少有人使用模型中毒攻击的方法。虽然krum、中位数采样等增强了攻击，但是降低了联合模型在主要任务中的准确性

\2.    未来异常检测与安全聚合可能会兼容，如何去证明模型替换攻击仍有效

## Tips

模型中毒攻击与训练数据中毒攻击的区别：模型是指带有后门功能的模型，而训练数据是投入中毒的数据来操纵客户端模型

# FDCR

 [FDCR_NeurIPS_2024.pdf](FDCR_NeurIPS_2024.pdf) 

摒弃恶意

## 问题

如何在联邦学习背景下（数据异构性）实现后门攻击的防御

## 动机

1.距离差异防御；2.统计分布防御；3.代理模型防御；4.客户端防御

1与2都是以数据同质性为前提，并没有考虑进异构性的条件。3需要寻找合适的验证数据集；4需要遵守同一优化规则，在自适应任务重容易受到攻击。

以异构网络为前提，根据良性分布与恶意分布不同的参数重要度区分良性客户端与恶意攻击者。

## 方法

1.Fisher Client Discrepancy Cluster

简化FIM信息矩阵：![img](https://gitee.com/he-wenwen1171571169/images/raw/master/clip_image002.jpg)

->归一化：![image-20241002195947477](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002195947477.png)

->参数梯度：![img](https://gitee.com/he-wenwen1171571169/images/raw/master/clip_image006.jpg)![image-20241002194024597](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002194024597.png)

->聚合梯度：![image-20241002194100336](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002194100336.png)

->梯度差异值：![image-20241002194113489](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002194113489.png)

->聚类->剔除恶意客户端

->Fisher Parameter Rescale Aggregation

# Krum

 [NIPS-2017-machine-learning-with-adversaries-byzantine-tolerant-gradient-descent-Paper.pdf](NIPS-2017-machine-learning-with-adversaries-byzantine-tolerant-gradient-descent-Paper.pdf) 

摒弃恶意

## 问题

解决分布式学习中拜占庭故障

## 动机

\1.   线性的方法不能容忍单个拜占庭工作者，因为单个拜占庭工作者会出现难以收敛的情况

\2.   非线性的方法在有两个拜占庭工作者的情况下一个帮助另一个被选中，通过移动所有向量的重心远离“正确区域”。

提出了Krum算法，一种满足拜占庭容错性质的非线性聚合规则。

## 方法

基于SGD

\1.   计算得分：![image-20241002201226275](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002201226275.png)，自己与其他工作者的向量差异之和

\2.  选择得分最低的向量作为梯度更新的方向：![img](https://gitee.com/he-wenwen1171571169/images/raw/master/clip_image004.jpg)

# FoolsGold

 [Mitigating Sybils in Federated Learning Poisoning.pdf](Mitigating Sybils in Federated Learning Poisoning.pdf) 

非摒弃

## 问题

如何实现联邦学习背景下应对模型中毒的防御方法

**注意：没有特别要求是后门攻击，文章提出的方法既能用在后门攻击上也能用在标签翻转攻击**

## 动机

应对集中式中毒攻击的方法：异常检测与鲁棒性损失都不适用于联邦学习

中毒防御对恶意攻击者的数量有限制

文章提出的方法既可以运用在联邦学习中，也可以对攻击者的数量没有限制

## 方法

基于SGD

1.历史更新累计与计算特征权重->计算相似度

![image-20241002170236317](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002170236317.png)

2.防止因偶然有高相似度而错误降级，设置一个置信度，大于这个置信度调整相似度

3.对每个客户端找出与其最大的相似度值

4.基于最大相似度值，计算每个客户端的学习率αi

![image-20241002170500299](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002170500299.png)

5.根据学习率更新参数

# SDFC

 [Fisher Calibration for Backdoor-Robust.pdf](Fisher Calibration for Backdoor-Robust.pdf) 

非摒弃

## 问题

在异构联邦学习网络中防御后门攻击

## 动机

现有防御方法有三类：

1.距离差防御 2.统计分布防御 3.模型细化防御

上述都基于两种规则：1.数据同质性 2.较小的恶意客户端比例

文章出发点在于希望在异构环境下即使在恶意客户端比例较高的情况下也能实现理想的防御效果。

## 方法

1.将本地客户端模型与干净验证集上的Fisher信息矩阵作差以评估本地更新对于全局目标的差异

2.聚合权重分配

客户端Loss更新：

![image-20241002193517404](https://gitee.com/he-wenwen1171571169/images/raw/master/image-20241002193517404.png)