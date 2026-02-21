#### 问题 3.8 伯努利分布的贝叶斯估计

**(a) 似然函数**

给定独立同分布样本 \(\mathcal{D} = \{x_1, \ldots, x_n\}\) 来自参数为 \(\pi\) 的伯努利分布，似然函数为：
\[
p(\mathcal{D} | \pi) = \prod_{i=1}^n p(x_i | \pi) = \prod_{i=1}^n \pi^{x_i} (1-\pi)^{1-x_i} = \pi^{\sum_{i=1}^n x_i} (1-\pi)^{n - \sum_{i=1}^n x_i} = \pi^s (1-\pi)^{n-s}
\]
其中 \(s = \sum_{i=1}^n x_i\) 是充分统计量。

**(b) 均匀先验下的后验分布**

假设均匀先验 \(p(\pi) = 1\)，其中 \(0 \leq \pi \leq 1\)。后验分布与似然乘以先验成比例：
\[
p(\pi | \mathcal{D}) \propto p(\mathcal{D} | \pi) p(\pi) = \pi^s (1-\pi)^{n-s}
\]
归一化常数由积分给出：
\[
\int_0^1 \pi^s (1-\pi)^{n-s} d\pi = \frac{\Gamma(s+1)\Gamma(n-s+1)}{\Gamma(n+2)}
\]
使用提供的恒等式。因此，后验分布为：
\[
p(\pi | \mathcal{D}) = \frac{\Gamma(n+2)}{\Gamma(s+1)\Gamma(n-s+1)} \pi^s (1-\pi)^{n-s}
\]
这是一个 Beta 分布，参数为 \(\alpha = s+1\) 和 \(\beta = n-s+1\)。

当 \(n=1\) 时，\(s\) 可能为 0 或 1：
- 如果 \(s=0\)，\(p(\pi | \mathcal{D}) = \frac{\Gamma(3)}{\Gamma(1)\Gamma(2)} \pi^0 (1-\pi)^1 = \frac{2!}{0! \cdot 1!} (1-\pi) = 2(1-\pi)\)
- 如果 \(s=1\)，\(p(\pi | \mathcal{D}) = \frac{\Gamma(3)}{\Gamma(2)\Gamma(1)} \pi^1 (1-\pi)^0 = \frac{2}{1 \cdot 1} \pi = 2\pi\)

绘图对于 \(n=1\)：
- 当 \(s=0\)：线性递减函数，从 \(\pi=0\) 处的 2 到 \(\pi=1\) 处的 0。
- 当 \(s=1\)：线性递增函数，从 \(\pi=0\) 处的 0 到 \(\pi=1\) 处的 2。

**(c) 预测分布**

新样本 \(x\) 的预测分布为：
\[
p(x | \mathcal{D}) = \int_0^1 p(x | \pi) p(\pi | \mathcal{D}) d\pi
\]
对于 \(x=1\)：
\[
p(x=1 | \mathcal{D}) = \int_0^1 \pi \cdot p(\pi | \mathcal{D}) d\pi
\]
由于 \(p(\pi | \mathcal{D})\) 是 Beta(\(s+1, n-s+1\)) 分布，其均值为：
\[
E[\pi | \mathcal{D}] = \frac{s+1}{(s+1) + (n-s+1)} = \frac{s+1}{n+2}
\]
因此，\(p(x=1 | \mathcal{D}) = \frac{s+1}{n+2}\).

\(\pi\) 的有效贝叶斯估计是后验均值 \(\frac{s+1}{n+2}\)。这可以解释为在 ML 估计中添加了两个虚拟样本，一个成功和一个失败。这被称为拉普拉斯平滑或加一平滑。

**(d) 均匀先验下的 ML 和 MAP 估计**

ML 估计最大化似然函数 \(p(\mathcal{D} | \pi)\)：
\[
\hat{\pi}_{ML} = \frac{s}{n}
\]
在均匀先验下，MAP 估计最大化后验分布，后验与似然成比例，因此：
\[
\hat{\pi}_{MAP} = \frac{s}{n}
\]
贝叶斯估计 \(\frac{s+1}{n+2}\) 相对于 ML 估计的优势在于当 \(n\) 较小时。例如，如果 \(n=1\) 且 \(s=0\)，\(\hat{\pi}_{ML} = 0\)，预测永远没有成功，但贝叶斯估计 \(\frac{1}{3}\) 更合理。均匀先验添加了虚拟样本，使估计对小样本更稳健。

**(e) 替代先验下的 MAP 估计**

考虑先验 \(p_1(\pi) = 2\pi\) 和 \(p_0(\pi) = 2(1-\pi)\)，其中 \(0 \leq \pi \leq 1\).

对于 \(p_1(\pi) = 2\pi\)：
后验为：
\[
p(\pi | \mathcal{D}) \propto p(\mathcal{D} | \pi) p_1(\pi) = \pi^s (1-\pi)^{n-s} \cdot 2\pi = 2 \pi^{s+1} (1-\pi)^{n-s}
\]
最大化对数后验：
\[
\frac{d}{d\pi} \left[ (s+1) \log \pi + (n-s) \log (1-\pi) \right] = \frac{s+1}{\pi} - \frac{n-s}{1-\pi} = 0
\]
求解：
\[
\frac{s+1}{\pi} = \frac{n-s}{1-\pi} \implies (s+1)(1-\pi) = (n-s)\pi \implies s+1 = \pi(n+1) \implies \hat{\pi}_{MAP} = \frac{s+1}{n+1}
\]

对于 \(p_0(\pi) = 2(1-\pi)\)：
后验为：
\[
p(\pi | \mathcal{D}) \propto \pi^s (1-\pi)^{n-s} \cdot 2(1-\pi) = 2 \pi^s (1-\pi)^{n-s+1}
\]
最大化对数后验：
\[
\frac{d}{d\pi} \left[ s \log \pi + (n-s+1) \log (1-\pi) \right] = \frac{s}{\pi} - \frac{n-s+1}{1-\pi} = 0
\]
求解：
\[
\frac{s}{\pi} = \frac{n-s+1}{1-\pi} \implies s(1-\pi) = (n-s+1)\pi \implies s = \pi(n+1) \implies \hat{\pi}_{MAP} = \frac{s}{n+1}
\]

有效贝叶斯估计：
- 对于 \(p_1\)，\(\hat{\pi}_{MAP} = \frac{s+1}{n+1}\) 添加了一个虚拟成功样本。
- 对于 \(p_0\)，\(\hat{\pi}_{MAP} = \frac{s}{n+1}\) 添加了一个虚拟失败样本。