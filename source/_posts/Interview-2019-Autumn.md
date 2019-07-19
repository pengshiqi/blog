---
title: Interview 2019 Autumn
date: 2019-07-05 09:08:06
tags: [Life, Interview]
categories: [Life]
---



我决定开个帖记录一下2019年秋天找工作面试中遇到的一些问题。

<!-- more -->



### 准备工作

**快速排序**

```c++
void qsort(vector<int> &a, int l, int r) {
    if (l >= r) return;

    int i = l, j = r, key = a[l];
    while (i < j) {
        while (i < j && a[j] >= key) --j;
        a[i] = a[j];
        while (i < j && a[i] <= key) ++i;
        a[j] = a[i];
    }
    a[i] = key;

    qsort(a, l, i - 1);
    qsort(a, i + 1, r);
}
```



**SVM 的推导**



**Logistic Regression 的推导**



**Bagging 与 Boosting**



**K-means 与 KNN**

```python
# K-means
def initCentroids(dataSet, k):
    # 从数据集中随机选取k个数据返回
    dataSet = list(dataSet)
    return random.sample(dataSet, k)

def minDistance(dataSet, centroidList):
    # 对每个属于dataSet的item， 计算item与centroidList中k个质心的距离，找出距离最小的，并将item加入相应的簇类中
    clusterDict = dict() #dict保存簇类结果
    k = len(centroidList)
    for item in dataSet:
        vec1 = item
        flag = -1
        minDis = float("inf") # 初始化为最大值
        for i in range(k):
            vec2 = centroidList[i]
            distance = calcuDistance(vec1, vec2)  # error
            if distance < minDis:
                minDis = distance
                flag = i  # 循环结束时， flag保存与当前item最近的蔟标记
        if flag not in clusterDict.keys():
            clusterDict.setdefault(flag, [])
        clusterDict[flag].append(item)  #加入相应的类别中
    return clusterDict  #不同的类别

def getCentroids(clusterDict):
    #重新计算k个质心
    centroidList = []
    for key in clusterDict.keys():
        centroid = np.mean(clusterDict[key], axis=0)
        centroidList.append(centroid)
    return centroidList  #得到新的质心

def getVar(centroidList, clusterDict):
    # 计算各蔟集合间的均方误差
    # 将蔟类中各个向量与质心的距离累加求和
    sum = 0.0
    for key in clusterDict.keys():
        vec1 = centroidList[key]
        distance = 0.0
        for item in clusterDict[key]:
            vec2 = item
            distance += calcuDistance(vec1, vec2)
        sum += distance
    return sum

def k_means(dataset k):
    centroidList = initCentroids(dataSet, k)
    clusterDict = minDistance(dataSet, centroidList)

    newVar = getVar(centroidList, clusterDict)
    oldVar = 1  # 当两次聚类的误差小于某个值是，说明质心基本确定。

    times = 2
    while abs(newVar - oldVar) >= 0.00001:
        centroidList = getCentroids(clusterDict)
        clusterDict = minDistance(dataSet, centroidList)
        oldVar = newVar
        newVar = getVar(centroidList, clusterDict)
        times += 1
        # showCluster(centroidList, clusterDict)
```



**Global Average Pooling 的前向传播与反向传播**

```c
void forward_avgpool_layer(const avgpool_layer l, network net)
{
    int b,i,k;

    for(b = 0; b < l.batch; ++b){
        for(k = 0; k < l.c; ++k){
            int out_index = k + b*l.c;
            l.output[out_index] = 0;
            for(i = 0; i < l.h*l.w; ++i){
                int in_index = i + l.h*l.w*(k + b*l.c);
                l.output[out_index] += net.input[in_index];
            }
            l.output[out_index] /= l.h*l.w;
        }
    }
}

void backward_avgpool_layer(const avgpool_layer l, network net)
{
    int b,i,k;

    for(b = 0; b < l.batch; ++b){
        for(k = 0; k < l.c; ++k){
            int out_index = k + b*l.c;
            for(i = 0; i < l.h*l.w; ++i){
                int in_index = i + l.h*l.w*(k + b*l.c);
                net.delta[in_index] += l.delta[out_index] / (l.h*l.w);
            }
        }
    }
}
```

reference: <https://github.com/pjreddie/darknet/blob/master/src/avgpool_layer.c>



**Batchnorm 的原理**

主要解决的问题： 

1. 上一层的输出数据经过这一层的网络后，数据分布发生了变化，为下一层的网络学习带来困难。

2. 训练数据和测试数据的分布存在差异性，给网络的泛化性能带来影响。

<img src="/images/Interview-2019-Autumn/batchnorm.png" style="zoom:40%" />

reference: <https://blog.csdn.net/qq_25737169/article/details/79048516>



**各种优化器的原理 (SGD, Adam)**

基本范式(待补充详细)：

1. 计算目标函数关于当前参数的梯度
2. 根据历史梯度计算一阶动量和二阶动量
3. 计算当前时刻的下降梯度
4. 根据下降梯度更新参数

reference: <https://zhuanlan.zhihu.com/p/32230623>



### 2019-07-05 字节跳动

上次被头条爆锤的场景依旧历历在目…..



讲一下 Mask RCNN 的结构，说一下 anchor，与YOLO、SSD中的 anchor 有什么不同。

讲一下 U-Net 的结构，PyTorch中的 upsample 怎么做的，有哪些参数。

你觉得 hour-glass (encoder-decoder)结构能带来什么好处。

转置卷积、卷积 的反向传播是怎么做的？



最后是一道编程题：

有n个节点，每个节点有一个数值，如果两个节点数值的最大公约数大于1，那么在这两个点之间连一条边。

求这个图的最大连通分量。