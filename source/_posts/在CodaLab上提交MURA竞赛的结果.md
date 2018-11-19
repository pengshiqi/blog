---
title: 在CodaLab上提交MURA竞赛的结果
date: 2018-06-25 20:15:42
tags: 
---



What is MURA?

>MURA (**mu**sculoskeletal **ra**diographs) is a large dataset of bone X-rays. Algorithms are tasked with determining whether an X-ray study is normal or abnormal.

这是斯坦福大学机器学习组的骨骼X光深度学习比赛(Bone X-Ray Deep Learning Competition)，这是它的[官网](https://stanfordmlgroup.github.io/competitions/mura/)。

数据集是七类人体部位的X光，分为normal和abnormal两种，需要训练一个模型，能够完成这样一个二分类的诊断任务。

Baseline是一个169层的DenseNet，我们用PyTorch实现了它。

答案的提交是通过CodaLab这个网站，官方给出了这样一个[提交教程](https://worksheets.codalab.org/worksheets/0x42dda565716a4ee08d61f0a23656d8c0/)：

<!--more-->

**Step 1:**

创建一个新的WorkSheet。



**Step 2:**

复制一份虚拟的数据：

```
cl add bundle mura-utils//valid .
cl add bundle mura-utils//valid_image_paths.csv .
```



**Step 3:**

将你的代码和模型打包后上传，压缩成zip后上传，系统会自动解压。

这份伪造数据的路径保存在`valid_image_paths.csv`中，格式为：

```
MURA-v1.1/{valid,test}/<STUDYTYPE>/<PATIENT>/<STUDY>/<IMAGE>
```

你的程序需要能够输出一份对于每一个study的预测结果，形如：

```
MURA-v1.1/{valid,test}/<STUDYTYPE>/<PATIENT>/<STUDY>/,{0,1}
```

 

**Step 4:**

在伪造数据集上运行你预训练好的模型。这是他们给出的运行命令：

```
cl run valid_image_paths.csv:valid_image_paths.csv MURA-v1.1:valid src:src "python src/<path-to-prediction-program> valid_image_paths.csv predictions.csv" -n run-predictions
```

 这里需要用到docker，我在docker hub准备了一个docker镜像，sqpeng1996/mura2:v5，里面有python 3.6，pytorch, torchnet, sklearn等代码需要的库。

使用命令为:

```
cl run valid_image_paths.csv:valid_image_paths.csv MURA-v1.1:valid MURA:MURA "python3 MURA/main.py valid_images_paths.csv predictions.csv" -n run-predictions --request-docker-image sqpeng1996/mura2:v5 --request-cpus 4 --request-memory 32g
```

这里特别要注意的是需要多请求一些memory，（或者把batch size改为1）,不然无法跑通。

如果这一步的state变成了ready，那就说明没问题了。然后执行：

```
cl make run-predictions/predictions.csv -n predictions-{MODELNAME}
```

MODELNAME不能包含空格，最好也避免特殊字符。

最后执行：

```
cl macro mura-utils/valid-eval-v1.1 predictions-{MODELNAME}
```

就能看到结果了（因为这里是伪造的数据，所以结果不重要）。



**Step 5：**

提交：

```
cl edit predictions-{MODELNAME} --tags mura-submit
```

把 **predictions-{MODELNAME} ** bundle的 URL发邮件给 jirvin16@stanford.edu 就可以了。