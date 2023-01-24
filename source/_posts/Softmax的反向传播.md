---
title: Softmax的反向传播
date: 2019-07-06 15:07:24
tags: [Machine Learning]
categories: 
- 学习
mathjax: true
---

这几天在手写CNN，记录一下这其中遇到的几个问题。

<!-- more -->

对于一个分类问题，一般的做法是对最后线性层的输出做一次softmax操作，然后再与one-hot形式的label计算交叉熵损失。

#### 1. softmax 函数的定义

设 $ X = [x_1, x_2, \dots, x_n]$，那么 $ Y = softmax(X) = [y1, y2, \dots, y_n] $，其中 $y_i = \frac{e^{x_i}}{\sum_{j=1}^n e^{x_j}}$，$\sum_{i=1}^n y_i = 1$。

在numpy实现中，为了避免浮点数溢出，需要将$X$中的元素减去$max(X)$:

```python
def softmax(x):
    """Compute softmax values for each sets of scores in x."""
    e_x = np.exp(x - np.expand_dims(np.max(x, axis=1), axis=1))
    return e_x / np.expand_dims(e_x.sum(axis=1), axis=1)
```

#### 2. softmax 函数的求导 $\frac{\partial{y_i}}{\partial{x_j}}$

(1) 当 $i = j$ 时，
$$
\begin{equation}
\begin{split}
\frac{\partial{y_i}}{\partial{x_j}} &= \frac{\partial{y_i}}{\partial{x_i}} \\
                              &= \frac{\partial{}}{\partial{x_i}}(\frac{e^{x_i}}{\sum_k{e^{x_k}}}) \\
                              &= \frac{e^{x_i}*(\sum_k{e^{x_k})} - e^{x_i}*e^{x_i}}{(\sum_k{e^{x_k}})^2} \\
                              &= \frac{e^{x_i}}{\sum_k{e^{x_k}}} - \frac{e^{x_i} * e^{x_i}}{(\sum_k{e^{x_k}})^2} \\
                              &= y_i - y_i^2 \\
                              &= y_i(1-y_i)
\end{split}
\end{equation}
$$


(2) 当 $i \neq j$ 时，
$$
\begin{equation}
\begin{split}
\frac{\partial{y_i}}{\partial{x_j}} &=  \frac{\partial{}}{\partial{x_j}}(\frac{e^{x_i}}{\sum_k{e^{x_k}}}) \\
                              &= - \frac{e^{x_j}*e^{x_i}}{(\sum_k{e^{x_k}})^2} \\
                              &= - \frac{e^{x_i}}{\sum_k{e^{x_k}}} * \frac{e^{x_j}}{\sum_k{e^{x_k}}} \\
                              &= - y_i y_j 
\end{split}
\end{equation}
$$
综上所述，
$$
\begin{equation}
\frac{\partial{y_i}}{\partial{x_j}} = \left\{  
             \begin{array}{**lr**}  
             y_i - y_i * y_i, & i = j\\  
             0 - y_i * y_j,   & i \neq j    
             \end{array}  
\right.
\end{equation}
$$

#### 3. softmax + cross-entropy

交叉熵损失函数： $\mathcal{L} = -\sum_k {\hat{y_k} \log y_k}$

其中 $\hat{y_k}$ 是标签，$y_k$是softmax函数的输出值。
$$
\begin{equation}
\begin{split}
\frac{\partial{\mathcal{L}}}{\partial{x_k}} &= \sum_i \frac{\partial{\mathcal{L}}}{\partial{y_i}} \frac{\partial{y_i}}{\partial{x_k}} \\
                                      &= - \sum_i \frac{\partial{(\hat{y_i} * \log y_i)}}{\partial{y_i}} \frac{\partial{y_i}}{\partial{x_k}} \\
                                      &= - \sum_i \frac{\hat{y_i}}{y_i} \frac{\partial{y_i}}{\partial{x_k}} \\
                                      &= - \frac{\hat{y_k}}{y_k} \frac{\partial{y_k}}{\partial{x_k}} - \sum_{i \neq k} \frac{\hat{y_i}}{y_i} \frac{\partial{y_i}}{\partial{x_k}} \\
                                      &= - \frac{\hat{y_k}}{y_k} (y_k - y_k^2) - \sum_{i \neq k}\frac{\hat{y_i}}{y_i} * (-y_i y_k) \\
                                      &= -\hat{y_k} (1-y_k) + \sum_{i \neq k} \hat{y_i} y_k \\
                                      &= -\hat{y_k} + \sum_i \hat{y_i} * y_k \\
                                      &= y_k - \hat{y_k}
\end{split}
\end{equation}
$$
最终的这个结果可以说是非常简单了。

