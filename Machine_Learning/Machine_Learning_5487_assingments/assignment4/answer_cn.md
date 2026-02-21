### 解答

#### (a) 证明分布 $\hat{p}(x)$ 的均值等于样本均值

分布 $\hat{p}(x)$ 的均值定义为：

\[
\hat{\mu} = \mathbb{E}_{\hat{p}}[x] = \int x \hat{p}(x) \, dx
\]

代入 $\hat{p}(x) = \frac{1}{n} \sum_{i=1}^{n} \tilde{k}(x - x_i)$：

\[
\hat{\mu} = \int x \left( \frac{1}{n} \sum_{i=1}^{n} \tilde{k}(x - x_i) \right) dx
\]

交换积分与求和顺序：

\[
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} \int x \tilde{k}(x - x_i) \, dx
\]

对于每个积分，令 $y = x - x_i$，则 $x = y + x_i$，$dx = dy$：

\[
\int x \tilde{k}(x - x_i) \, dx = \int (y + x_i) \tilde{k}(y) \, dy = \int y \tilde{k}(y) \, dy + x_i \int \tilde{k}(y) \, dy
\]

由核函数性质：$\int \tilde{k}(y) \, dy = 1$ 和 $\mathbb{E}_{\tilde{k}}[y] = \int y \tilde{k}(y) \, dy = 0$，因此：

\[
\int x \tilde{k}(x - x_i) \, dx = 0 + x_i \cdot 1 = x_i
\]

代入得：

\[
\hat{\mu} = \frac{1}{n} \sum_{i=1}^{n} x_i
\]

证毕。

#### (b) 证明分布 $\hat{p}(x)$ 的协方差为给定形式

分布 $\hat{p}(x)$ 的协方差定义为：

\[
\hat{\Sigma} = \text{cov}_{\hat{p}}(x) = \int \hat{p}(x) (x - \hat{\mu}) (x - \hat{\mu})^T dx
\]

代入 $\hat{p}(x)$：

\[
\hat{\Sigma} = \int (x - \hat{\mu}) (x - \hat{\mu})^T \left( \frac{1}{n} \sum_{i=1}^{n} \tilde{k}(x - x_i) \right) dx = \frac{1}{n} \sum_{i=1}^{n} \int (x - \hat{\mu}) (x - \hat{\mu})^T \tilde{k}(x - x_i) dx
\]

对于每个积分，令 $y = x - x_i$，则 $x = y + x_i$，$dx = dy$：

\[
\int (x - \hat{\mu}) (x - \hat{\mu})^T \tilde{k}(x - x_i) dx = \int (y + x_i - \hat{\mu}) (y + x_i - \hat{\mu})^T \tilde{k}(y) dy
\]

展开被积函数：

\[
(y + x_i - \hat{\mu}) (y + x_i - \hat{\mu})^T = y y^T + y (x_i - \hat{\mu})^T + (x_i - \hat{\mu}) y^T + (x_i - \hat{\mu}) (x_i - \hat{\mu})^T
\]

逐项积分：

- $\int y y^T \tilde{k}(y) dy = H$（由核函数协方差定义）
- $\int y (x_i - \hat{\mu})^T \tilde{k}(y) dy = \left( \int y \tilde{k}(y) dy \right) (x_i - \hat{\mu})^T = 0 \cdot (x_i - \hat{\mu})^T = 0$
- $\int (x_i - \hat{\mu}) y^T \tilde{k}(y) dy = (x_i - \hat{\mu}) \left( \int y^T \tilde{k}(y) dy \right) = (x_i - \hat{\mu}) \cdot 0 = 0$
- $\int (x_i - \hat{\mu}) (x_i - \hat{\mu})^T \tilde{k}(y) dy = (x_i - \hat{\mu}) (x_i - \hat{\mu})^T \int \tilde{k}(y) dy = (x_i - \hat{\mu}) (x_i - \hat{\mu})^T$

因此，积分结果为：

\[
\int (x - \hat{\mu}) (x - \hat{\mu})^T \tilde{k}(x - x_i) dx = H + (x_i - \hat{\mu}) (x_i - \hat{\mu})^T
\]

代入 $\hat{\Sigma}$：

\[
\hat{\Sigma} = \frac{1}{n} \sum_{i=1}^{n} \left[ H + (x_i - \hat{\mu}) (x_i - \hat{\mu})^T \right] = H + \frac{1}{n} \sum_{i=1}^{n} (x_i - \hat{\mu}) (x_i - \hat{\mu})^T
\]

证毕。

#### (c) 分析性质与偏置的关联

上述结果表明核密度估计 $\hat{p}(x)$ 具有以下性质：

1. **均值性质**：从 (a) 可知，$\hat{p}(x)$ 的均值 $\hat{\mu}$ 等于样本均值，这意味着在估计总体均值时，核密度估计是一阶无偏的。即，作为均值估计量，$\hat{\mu}$ 是真实均值的无偏估计。

2. **协方差性质**：从 (b) 可知，$\hat{p}(x)$ 的协方差 $\hat{\Sigma}$ 包含两项：核函数的协方差 $H$ 和样本协方差。这表明在估计总体协方差时，核密度估计是有偏的，偏置为 $H$。具体地，$\mathbb{E}[\hat{\Sigma}] = H + \Sigma$（其中 $\Sigma$ 是真实协方差），因此偏置为 $H$。

3. **与核密度估计量偏置的关联**：这些性质反映了核密度估计的平滑效应。核函数的使用引入了平滑，导致估计分布 $\hat{p}(x)$ 比经验分布更分散。在密度估计本身，偏置通常取决于带宽（隐含在 $H$ 中）：当带宽较大时，平滑程度高，方差减小但偏置增大（尤其在对密度函数点估计时）；当带宽较小时，平滑程度低，偏置减小但方差增大。这里从矩的角度表明，核密度估计对一阶矩无偏，但对二阶矩有偏，且偏置直接由核的协方差 $H$ 决定。因此，在实际应用中，需要权衡带宽选择以控制整体偏置和方差。

总之，核密度估计在均值估计上无偏，但在方差估计上存在正偏置，这源于核函数的平滑特性。