# Maximum Likelihood Estimation
# 极大似然估计

## Problem 2.1 Poisson Distribution and Flying Bombs (V-1 and V-2)
## 问题2.1 泊松分布与飞弹（V-1和V-2飞弹）

During World War II, the Germans launched V-1 and V-2 flying bombs (long-range missiles) at London. Some areas were hit more frequently than others, and the British military wanted to know if the multiple hits were because the Germans were targeting specific areas or purely due to chance.
在第二次世界大战期间，德国人向伦敦发射V-1和V-2飞弹（远程导弹）。有些区域被击中的次数比其他区域多，英国军方想知道多次击中是因为德国人针对特定区域瞄准，还是纯粹出于偶然。

To analyze this problem, British statistician R.D. Clarke divided a 144 square kilometer area into a regular grid, forming 576 cells. Under the assumption that bombs fell randomly, the probability of any cell being hit was constant across all cells. Therefore, the number of hits in each cell is an independent and identically distributed (i.i.d) sample from a common random variable \( x \).
为了分析这个问题，英国统计学家R.D.克拉克将一个144平方公里的区域划分为规则网格，形成576个单元格。在飞弹随机落下的假设下，任何单元格被击中的概率在所有单元格中都是恒定的。因此，各单元格的击中次数是来自共同随机变量\( x \)的独立同分布（i.i.d）样本。

A natural distribution for modeling the number of events (bomb hits) occurring in a fixed time interval is the Poisson distribution, given by:
用于建模固定时间段内发生的事件数（炸弹击中数）的一个自然分布是泊松分布，由下式给出：

\[ p(x = k \mid \lambda) = \frac{1}{k!} e^{-\lambda} \lambda^k \tag{2.1} \]

where \( k \in \{0, 1, 2, 3, \cdots\} \) is the count. The parameter \( \lambda \) is the average number of events, and the mean and variance are equal: \( \mathbb{E}[x] = \text{var}(x) = \lambda \).
其中\( k \in \{0, 1, 2, 3, \cdots\} \)是计数数。参数\( \lambda \)是事件的平均数量，且均值和方差相同：\( \mathbb{E}[x] = \text{var}(x) = \lambda \)。

(a) Given a set of i.i.d samples \( \{k_1, \cdots, k_N\} \), derive the maximum likelihood estimator for \( \lambda \).
(a) 给定一组i.i.d样本\( \{k_1, \cdots, k_N\} \)，推导\( \lambda \)的极大似然估计。

(b) Show that the maximum likelihood (ML) estimator is unbiased and that the variance of the estimator is \( \frac{\lambda}{N} \).
(b) 证明极大似然（ML）估计量是无偏的，且估计量的方差为\( \frac{\lambda}{N} \)。

The following table lists the number of cells observed to have \( k \) hits (this is Clarke's actual data!).
下表列出了观察到有\( k \)次击中的单元格数量（这是克拉克的实际数据！）。

| Number of hits \( k \) | 0   | 1   | 2   | 3   | 4   | 5 and above |
|------------------------|-----|-----|-----|-----|-----|-------------|
| Number of cells with \( k \) hits | 229 | 211 | 93  | 35  | 7   | 1           |
| 击中次数\( k \) | 0   | 1   | 2   | 3   | 4   | 5及以上 |
| 有\( k \)次击中的单元格数 | 229 | 211 | 93  | 35  | 7   | 1       |

(c) Using the above data, compute the ML estimate \( \hat{\lambda} \) for the Poisson distribution.
(c) 使用上述数据，计算泊松分布的ML估计\( \hat{\lambda} \)。

(d) Using the estimate \( \hat{\lambda} \), predict the expected number of cells with \( k \) hits (\( k \in \{0, 1, 2, 3, 4, 5+\} \)). Compare the expected counts with the observed data. What conclusions can be drawn?
(d) 使用估计\( \hat{\lambda} \)预测有\( k \)次击中的单元格的期望数量（\( k \in \{0, 1, 2, 3, 4, 5+\} \)）。将期望计数与观察数据进行比较。可以得出什么结论？


### Solutions
### 解答


#### (a) Deriving the maximum likelihood estimator for \( \lambda \)
#### (a) 推导\( \lambda \)的极大似然估计

The core of maximum likelihood estimation is to construct the likelihood function and find its maximum value.
极大似然估计的核心是构造似然函数并求其最大值。

1. **Construct the likelihood function**:
   Since the samples \( k_1, k_2, \dots, k_N \) are independent and identically distributed, the likelihood function is the product of the probabilities of each sample:
1. **构造似然函数**：
   由于样本\( k_1, k_2, \dots, k_N \)独立同分布，似然函数为各样本概率的乘积：

   \[
   \mathcal{L}(\lambda) = \prod_{i=1}^N \frac{e^{-\lambda} \lambda^{k_i}}{k_i!}
   \]

2. **Take the log-likelihood function**:
   Log-likelihood function (simplifies differentiation):
2. **取对数似然函数**：
   对数似然函数（简化求导）：

   \[
   \ell(\lambda) = \sum_{i=1}^N \left( -\lambda + k_i \ln \lambda - \ln(k_i!) \right) = -N\lambda + \left( \sum_{i=1}^N k_i \right) \ln \lambda - \sum_{i=1}^N \ln(k_i!)
   \]

3. **Differentiate with respect to \( \lambda \) and set the derivative to zero**:
   Taking the derivative:
3. **对\( \lambda \)求导并令导数为0**：
   求导得：

   \[
   \frac{d\ell(\lambda)}{d\lambda} = -N + \frac{1}{\lambda} \sum_{i=1}^N k_i
   \]

   Setting the derivative to zero and solving:
   令导数为0，解得：

   \[
   \lambda = \frac{1}{N} \sum_{i=1}^N k_i
   \]

4. **Verify maximum value**:
   The second derivative \( \frac{d^2\ell(\lambda)}{d\lambda^2} = -\frac{1}{\lambda^2} \sum_{i=1}^N k_i < 0 \), so this is a maximum point.
4. **验证极大值**：
   二阶导数\( \frac{d^2\ell(\lambda)}{d\lambda^2} = -\frac{1}{\lambda^2} \sum_{i=1}^N k_i < 0 \)，故为极大值点。

Therefore, the maximum likelihood estimator for \( \lambda \) is the **sample mean**:
因此，\( \lambda \)的极大似然估计为**样本均值**：

\[
\hat{\lambda}_{\text{ML}} = \frac{1}{N} \sum_{i=1}^N k_i
\]


#### (b) Show that the ML estimator is unbiased with variance \( \frac{\lambda}{N} \)
#### (b) 证明ML估计量无偏且方差为\( \frac{\lambda}{N} \)

- **Unbiasedness**:
  We know \( \hat{\lambda}_{\text{ML}} = \frac{1}{N} \sum_{i=1}^N k_i \), and each \( k_i \sim \text{Poisson}(\lambda) \), so \( \mathbb{E}[k_i] = \lambda \).
  By linearity of expectation:
- **无偏性**：
  已知\( \hat{\lambda}_{\text{ML}} = \frac{1}{N} \sum_{i=1}^N k_i \)，且每个\( k_i \sim \text{Poisson}(\lambda) \)，故\( \mathbb{E}[k_i] = \lambda \)。
  由期望的线性性：

  \[
  \mathbb{E}[\hat{\lambda}_{\text{ML}}] = \frac{1}{N} \sum_{i=1}^N \mathbb{E}[k_i] = \frac{1}{N} \cdot N\lambda = \lambda
  \]

  Therefore, the ML estimator is **unbiased**.
  因此，ML估计量是**无偏的**。

- **Variance**:
  Since the samples are independent and identically distributed, \( \text{var}(k_i) = \lambda \) and the covariance \( \text{cov}(k_i, k_j) = 0 \) for \( i \neq j \).
  By properties of variance:
- **方差**：
  由于样本独立同分布，\( \text{var}(k_i) = \lambda \)且协方差\( \text{cov}(k_i, k_j) = 0 \)（\( i \neq j \)）。
  由方差的性质：

  \[
  \text{var}(\hat{\lambda}_{\text{ML}}) = \text{var}\left( \frac{1}{N} \sum_{i=1}^N k_i \right) = \frac{1}{N^2} \sum_{i=1}^N \text{var}(k_i) = \frac{1}{N^2} \cdot N\lambda = \frac{\lambda}{N}
  \]


#### (c) Calculate the ML estimate \( \hat{\lambda} \)
#### (c) 计算ML估计\( \hat{\lambda} \)

Total number of cells \( N = 229 + 211 + 93 + 35 + 7 + 1 = 576 \).
总单元格数\( N = 229 + 211 + 93 + 35 + 7 + 1 = 576 \)。

Calculate total number of hits \( \sum_{i=1}^N k_i \) ("5 and above" approximated as \( k=5 \)):
计算总击中次数\( \sum_{i=1}^N k_i \)（“5及以上”近似为\( k=5 \)）：

\[
\begin{align*}
\sum k_i &= 0 \times 229 + 1 \times 211 + 2 \times 93 + 3 \times 35 + 4 \times 7 + 5 \times 1 \\
&= 0 + 211 + 186 + 105 + 28 + 5 = 535
\end{align*}
\]

Therefore, the ML estimate:
因此，ML估计：

\[
\hat{\lambda} = \frac{1}{N} \sum k_i = \frac{535}{576} \approx 0.929
\]


#### (d) Predict expected cell counts and compare
#### (d) 预测期望单元格数并比较

The probability mass function of the Poisson distribution is \( p(k \mid \hat{\lambda}) = \frac{e^{-\hat{\lambda}} \hat{\lambda}^k}{k!} \), and the expected number of cells is \( N \times p(k \mid \hat{\lambda}) \).
泊松分布的概率质量函数为\( p(k \mid \hat{\lambda}) = \frac{e^{-\hat{\lambda}} \hat{\lambda}^k}{k!} \)，期望单元格数为\( N \times p(k \mid \hat{\lambda}) \)。

Calculate \( e^{-\hat{\lambda}} \approx e^{-0.929} \approx 0.395 \), then compute expected counts for each \( k \):
计算\( e^{-\hat{\lambda}} \approx e^{-0.929} \approx 0.395 \)，然后对每个\( k \)计算期望数：

| \( k \)   | Observed \( O \) | Expected \( E \) (calculation) | Final \( E \approx \) | \( k \)   | 观察值\( O \) | 期望值\( E \)（计算过程）| 最终期望\( E \approx \) |
|-----------|--------------|------------------------------------------|------------------------|-----------|--------------|------------------------------------------|------------------------|
| 0         | 229          | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^0}{0!} \) | 227.5                  | 0         | 229          | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^0}{0!} \) | 227.5                  |
| 1         | 211          | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^1}{1!} \) | 210.8                  | 1         | 211          | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^1}{1!} \) | 210.8                  |
| 2         | 93           | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^2}{2!} \) | 98.1                   | 2         | 93           | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^2}{2!} \) | 98.1                   |
| 3         | 35           | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^3}{3!} \) | 30.3                   | 3         | 35           | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^3}{3!} \) | 30.3                   |
| 4         | 7            | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^4}{4!} \) | 7.0                    | 4         | 7            | \( 576 \times \frac{e^{-0.929} \cdot (0.929)^4}{4!} \) | 7.0                    |
| 5+        | 1            | \( 576 \times \left( 1 - \sum_{k=0}^4 p(k \mid \hat{\lambda}) \right) \) | 2.3                    | 5及以上   | 1            | \( 576 \times \left( 1 - \sum_{k=0}^4 p(k \mid \hat{\lambda}) \right) \) | 2.3                    |


**Conclusions**:
- Observations for low hit counts (\( k=0,1 \)) are very close to expected values, indicating the Poisson distribution ("purely random" hypothesis) fits well.
- The observation for \( k=2 \) is slightly lower than expected, the observation for \( k=3 \) is significantly higher than expected, and observations for \( k \geq 5 \) are lower than expected.
- Overall, bomb hits largely conform to a "random distribution," but there are slight deviations (e.g., slightly more hits for \( k=3 \)), requiring further analysis to confirm if non-random factors exist.
**结论**：
- 低击中次数（\( k=0,1 \)）的观察值与期望值非常接近，说明泊松分布（“纯粹随机”假设）能较好拟合。
- \( k=2 \)的观察值略低于期望，\( k=3 \)的观察值明显高于期望，\( k \geq 5 \)的观察值低于期望。
- 整体而言，飞弹击中在很大程度上符合“随机分布”，但存在轻微偏差（如\( k=3 \)的击中次数稍多），需进一步分析确认是否存在非随机因素。


### Final Answers
### 最终答案

(a) The maximum likelihood estimator is \( \hat{\lambda} = \frac{1}{N} \sum_{i=1}^N k_i \);
(a) 极大似然估计为\( \hat{\lambda} = \frac{1}{N} \sum_{i=1}^N k_i \)；

(b) Unbiasedness and variance are proven using linearity of expectation and properties of variance respectively;
(b) 无偏性和方差分别通过期望线性性与方差性质证明；

(c) \( \hat{\lambda} \approx 0.929 \);
(c) \( \hat{\lambda} \approx 0.929 \)；

(d) Expected cell counts are generally consistent with observations, and the Poisson distribution (random hypothesis) fits the data well with small deviations.
(d) 期望单元格数与观察值大致符合，泊松分布（随机假设）能较好拟合数据，但存在小偏差。