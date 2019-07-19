---
title: EM Algorithm
date: 2018-01-18 13:08:13
tags: [Machine Learning]
categories: [Machine Learning]
mathjax: true
---

I'd like to talk about something about EM algorithm in my understanding.

This post is mainly based on [Richard Xu's machine learning course](https://www.bilibili.com/video/av12802062/?from=search&seid=12077428943005827344).

<!-- more -->

### Gaussian Mixture Model

Gaussian Mixture Model (GMM) (k-mixture) is defined as:
$$
p(X | \Theta) = \sum_{l=1}^k \alpha_l \mathcal{N}(X | \mu_l, \Sigma_l) \tag{1}
$$

$$
\sum_{l=1}^k \alpha_l = 1 \tag{2}
$$

and
$$
\Theta = \{ \alpha_1, \dots, \alpha_k, \mu_1, \dots, \mu_k, \Sigma_1, \dots, \Sigma_k \} \tag{3}
$$

For data $X = \{ x_1, \dots, x_n \}$, we introduce latent variable $Z = \{ z_1, \dots, z_n \}$, each $z_i$ indicates which mixture components $x_i$ belongs to. (The introduction of latent variable should **not** change the *marginal distribution* of $p(X)$.)

Then we can use MLE to estimate $\Theta$ : 
$$
\Theta_{MLE} = \mathop{\arg\max}\limits_{\Theta} \Big( \sum_{i=1}^N \log \big[ \sum_{l=1}^k \alpha_l \mathcal{N}(x_i | \mu_l, \Sigma_l) \big] \Big) \tag{4} 
$$

This formula is difficult to solve because it is in 'log-of-sum' form. So, we solve this problem in an iterative way, called ***Expectation Maximization***.


### Expectation Maximization

Instead of performing 
$$
\Theta_{MLE} = \mathop{\arg\max}\limits{\Theta} \Big( \mathcal{L}(\Theta) \Big) = \mathop{\arg\max}\limits_{\Theta} \Big( \log \big( p(X|\Theta) \big ) \Big) \tag{5}
$$

we assume some latent variable $Z$ to the model, such that we generate a series of $\Theta = \{ \Theta^{(1)},  \Theta^{(2)}, \dots, \Theta^{(t)} \}$.

For each iteration of the E-M algorithm, we perform:
$$
\Theta^{(g+1)} =\mathop{\arg\max}\limits_{\Theta} \Big( \int_Z \log \big( p(X, Z | \Theta) p (Z | X, \Theta^{(g)}) \big) \Big) dZ \tag{6}
$$

We must ensure convergence:
$$
\log p(X|\Theta^{(g+1)}) \ge \log p(X|\Theta^{(g)}) \tag{7}
$$
***Proof*** :
$$
E_{p(Z|X, \Theta^{(g)})} \Big[\log p(X|\Theta) \Big] = E_{p(Z|X, \Theta^{(g)})} \Big[\log p(X,Z|\Theta) - \log p(Z|X,\Theta) \Big] \tag{8}
$$

$$
\log p(X|\Theta) =  \int_Z  \log p(X,Z|\Theta) p(Z|X, \Theta^{(g)}) dZ -  \int_Z  \log p(Z|X,\Theta) p(Z|X, \Theta^{(g)}) dZ \tag{9}
$$

denote 
$$
Q(\Theta, \Theta^{(g)}) = \int_Z  \log p(X,Z|\Theta) p(Z|X, \Theta^{(g)}) dZ \\
H(\Theta, \Theta^{(g)}) = \int_Z  \log p(Z|X,\Theta) p(Z|X, \Theta^{(g)}) dZ
$$
then we have
$$
\log p(X|\Theta) =Q(\Theta, \Theta^{(g)}) - H(\Theta, \Theta^{(g)}) \tag{10}
$$
Because 
$$
Q(\Theta^{(g)}, \Theta^{(g)}) \le Q(\Theta^{(g+1)}, \Theta^{(g)}) \\
H(\Theta^{(g)}, \Theta^{(g)}) \ge H(\Theta^{(g+1)}, \Theta^{(g)})
$$
the second inequality can be derived using Jensen's inequality.

Hence , 
$$
\log p(X|\Theta^{(g+1)}) \ge \log p(X|\Theta^{(g)}) \tag{11}
$$

### Using EM algorithm to solve GMM

Put GMM into this frame work.
$$
\Theta^{(g+1)} = \mathop{\arg\max}\limits_{\Theta} \big[ Q(\Theta, \Theta^{(g)}) \big] =\mathop{\arg\max}\limits_{\Theta} \Big( \int_Z \log \big( p(X, Z | \Theta) p (Z | X, \Theta^{(g)}) \big) \Big) dZ \tag{12}
$$
***E-Step:***

Define $ p(X, Z | \Theta)$ :
$$
p(X, Z | \Theta) = \Pi_{i=1}^n p(x_i, z_i | \Theta) = \Pi_{i=1}^n p(x_i|z_i, \Theta) p(z_i|\Theta) =  \Pi_{i=1}^n \alpha_{z_i} \mathcal{N}(\mu_{z_i}, \Sigma_{z_i}) \tag{13}
$$
Define $p (Z | X, \Theta)$ :
$$
p (Z | X, \Theta) = \Pi_{i=1}^{n} p(z_i|x_i, \Theta) = \Pi_{i=1}^{n} \frac{\alpha_{z_i} \mathcal{N} (\mu_{z_i}, \Sigma_{z_i})}{\sum_{l=1}^k \alpha_l \mathcal{N} (\mu_l, \Sigma_l)} \tag{14}
$$
 Then 
$$
Q(\Theta, \Theta^{(g)}) = \sum_{z_1= 1}^k  \sum_{z_2= 1}^k  \dots  \sum_{z_N= 1}^k \Big( \sum_{i=1}^N \big[ \log \alpha_{z_i} + \log \mathcal{N}(\mu_{z_i}, \Sigma_{z_i})  \big] * \Pi_{i=1}^{N}  p(z_i| x_i, \Theta^{(g)}) \Big) 
$$
$$
 = \sum_{i=1}^N \sum_{l = 1}^k \big( \log \alpha_{l} + \log \mathcal{N}(\mu_{l}, \Sigma_{l}) \big) p(l| x_i, \Theta^{(g)}) \tag{15}
$$


***M-Step:***
$$
Q(\Theta, \Theta^{(g)}) = \sum_{i=1}^N \sum_{l = 1}^k  \log (\alpha_{l}) p(l| x_i, \Theta^{(g)})  + \sum_{i=1}^N \sum_{l = 1}^k\log \mathcal{N}(\mu_{l}, \Sigma_{l}) p(l| x_i, \Theta^{(g)}) \tag{16}
$$
The first term contains only $\alpha$ and the second term contains only $\mu, \Sigma$, so we can maximize both terms independantly.

Maximizing $\alpha$ means that:
$$
\frac{\partial \sum_{i=1}^N \sum_{l = 1}^k  \log (\alpha_{l}) p(l| x_i, \Theta^{(g)})}{\partial\alpha_1 \dots \partial\alpha_k} = 0 \tag{17}
$$
subject to $\sum_{l=1}^k = 1$. 

Solving this problem via Lagrangian Multiplier, we have
$$
\alpha_l = \frac{1}{N} \sum_{i=1}^N p(l| x_i, \Theta^{(g)}) \tag{18}
$$
Similarly, we can solve $\mu$ and $\Sigma$.

 












