# 题目5.2 核密度估计的均值与方差
在本题中，我们将研究核密度估计（即分布$\hat{p}(x)$）的均值与方差。设$X = \{x_1, \cdots, x_n\}$为样本集合，$\tilde{k}(x)$为包含带宽的核函数。其估计概率分布为：
$$\hat{p}(x)=\frac{1}{n} \sum_{i=1}^{n} \tilde{k}\left(x-x_{i}\right)$$

假设核函数$\tilde{k}(x)$满足零均值且协方差为$H$，即：
$$\mathbb{E}_{\tilde{k}}[x]=\int \tilde{k}(x) x d x=0$$
$$cov_{\tilde{k}}(x)=\int \tilde{k}(x)\left( x-\mathbb{E}_{\tilde{k}}[x]\right) \left( x-\mathbb{E}_{\tilde{k}}[x]\right) ^{T}dx=H \tag{5.7}$$


## (a) 证明
证明分布$\hat{p}(x)$的均值等于$X$的样本均值，即：
$$\hat{\mu}=\mathbb{E}_{\hat{p}}[x]=\int \hat{p}(x) x d x=\frac{1}{n} \sum_{i=1}^{n} x_{i}$$


## (b) 证明
证明分布$\hat{p}(x)$的协方差为：
$$\hat{\sum}=cov_{\hat{p}}(x)=H+\frac{1}{n} \sum_{i=1}^{n}\left(x_{i}-\hat{\mu}\right)\left(x_{i}-\hat{\mu}\right)^{T}$$
其中，等式右侧第二项为样本协方差。


## (c) 分析
上述结果表明核密度估计$\hat{p}(x)$具有哪些性质？该性质与核密度估计量的偏置有何关联？

