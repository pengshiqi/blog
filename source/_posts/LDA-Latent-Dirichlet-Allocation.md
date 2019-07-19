---
title: LDA(Latent Dirichlet Allocation)
date: 2017-09-14 15:54:31
tags: Machine Learning
categories: Machine Learning
mathjax: true
---

先借用Wikipedia上内容介绍一下LDA(隐含狄利克雷分布)：

>**隐含狄利克雷分布**简称LDA(Latent Dirichlet allocation)，是一种[主题模型](https://zh.wikipedia.org/wiki/%E4%B8%BB%E9%A2%98%E6%A8%A1%E5%9E%8B)，它可以将文档集中每篇文档的主题按照[概率分布](https://zh.wikipedia.org/wiki/%E6%A6%82%E7%8E%87%E5%88%86%E5%B8%83)的形式给出。同时它是一种[无监督学习](https://zh.wikipedia.org/wiki/%E9%9D%9E%E7%9B%A3%E7%9D%A3%E5%BC%8F%E5%AD%B8%E7%BF%92)算法，在训练时不需要手工标注的训练集，需要的仅仅是文档集以及指定主题的数量k即可。此外LDA的另一个优点则是，对于每一个主题均可找出一些词语来描述它。
>
>LDA是一种典型的词袋模型，即它认为一篇文档是由一组词构成的一个集合，词与词之间没有顺序以及先后的关系。一篇文档可以包含多个主题，文档中每一个词都由其中的一个主题生成。

<!-- more -->

理解LDA，可分为以下5个步骤：

1. 一个函数：gamma函数
2. 四个分布：二项分布、多项分布、beta分布、Dirichlet分布
3. 一个概念和一个理念：共轭先验和贝叶斯框架
4. 两个模型：pLSA、LDA
5. 一个采样：Gibbs采样



在LDA模型中，一篇文档的生成方式如下：

- 从狄利克雷分布 $\alpha$中取样生成文档 $i$  的主题分布 $\theta\_{i}$ 
- 从主题的多项式分布 $\theta\_{i}$ 中取样生成文档 $i$ 第 $j$ 个词的主题 $z\_{i,j}$ 
- 从狄利克雷分布 $\beta$ 中取样生成主题 $z\_{i,j}$ 对应的词语分布 $\phi\_{z\_{i,j}}$ 
- 从词语的多项式分布 $\phi\_{z\_{i,j}}$ 中采样最终生成词语 $w\_{i,j}$ .


## Reference

* [通俗理解LDA主题模型](http://blog.csdn.net/v_july_v/article/details/41209515)
* [隐含狄利克雷分布](https://zh.wikipedia.org/wiki/隐含狄利克雷分布)



