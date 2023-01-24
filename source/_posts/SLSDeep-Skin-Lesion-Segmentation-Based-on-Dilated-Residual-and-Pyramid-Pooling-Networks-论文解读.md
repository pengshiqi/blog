---
title: SLSDeep-Skin Lesion Segmentation Based on Dilated Residual and Pyramid Pooling Networks 论文解读
date: 2019-02-20 13:11:08
tags: 
- Image Segmentation
- MICCAI
categories: 
- 学习
---

这是发表在 MICCAI 2018 上的一篇关于病灶分割的文章。

arxiv链接在[https://arxiv.org/abs/1805.10241](https://arxiv.org/abs/1805.10241)，作者的开源代码在 [https://github.com/xmichelleshihx/SLSDeep](https://github.com/xmichelleshihx/SLSDeep) 。

<!-- more -->

-----

## Motivations

作者的任务是做 skin lesion segmentation (SLS)，采用了很常见的 encoder-decoder 的网络结构。

文章的主要贡献点：

1. 提出了一个 encoder(DRN) - decoder(PPN) 的网络结构；
2. 提出了一个新的 loss function (NLL loss + EPE loss)。

## Method

<img src="image1.png" style="zoom:100%" />

文章提出的网络结构如上图所示。

更深入的细节可以查看 Dilated Residual Network 的[论文](http://openaccess.thecvf.com/content_cvpr_2017/papers/Yu_Dilated_Residual_Networks_CVPR_2017_paper.pdf)和[代码](https://github.com/fyu/drn/blob/master/drn.py)，以及Pyramid Scene Parsing Network的[论文](https://arxiv.org/pdf/1612.01105.pdf)和[代码](https://github.com/Lextal/pspnet-pytorch)。

作者提出同时考虑 NLL loss 和 EPE loss，分别作为 objective loss 和 content loss，NLL loss其实就是 cross entropy loss 在二分类时的情况。

NLL loss，$v$是true label，$p$是probability estimate:

<img src="image2.png" style="zoom:50%" />

EPE loss 参考了IJCV 2011的[论文](http://vision.middlebury.edu/flow/floweval-ijcv2011.pdf)，$u$是 generated mask，$v$是 ground truth:

<img src="image3.png" style="zoom:50%" />

total loss 为两者加权相加：

<img src="image4.png" style="zoom:50%" />

## Experiments

作者在ISBI 2016和ISBI2017两个数据集上做了实验。

<img src="image5.png" style="zoom:60%" />

可以看出，加上EPE loss后，在accuracy上会有 1-2% 的提升，在dice上会有 4-5% 的提升。

<img src="image6.png" style="zoom:100%" />

这是一个qualitative result，展示了模型好的分割结果以及不好的分割结果。

## Conclusion

这篇文章想法比较简单，但我们可以通过这篇文章去了解CVPR 2017的两篇文章，Dilated Residual Network和Pyramid Scene Parsing Network，以及End Point Error loss，这是我们可以学习借鉴的东西。

