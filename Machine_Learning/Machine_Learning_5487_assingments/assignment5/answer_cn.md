### (a) 朋友报告正面（\(r=H\)）时的最优决策函数

给定朋友报告正面（\(r=H\)），我们需要在最小错误概率意义下决策真实结果 \(s\) 是 \(H\) 还是 \(T\)。根据贝叶斯决策理论，最优决策规则是比较后验概率 \(P(s=H | r=H)\) 和 \(P(s=T | r=H)\)，并选择后验概率较大的一方。

利用贝叶斯定理，后验概率之比正比于似然与先验的乘积：
\[
P(s=H | r=H) \propto P(r=H | s=H) P(s=H) = (1 - \theta_1) \alpha
\]
\[
P(s=T | r=H) \propto P(r=H | s=T) P(s=T) = \theta_2 (1 - \alpha)
\]
其中 \(P(s=H) = \alpha\)，\(P(s=T) = 1 - \alpha\)，\(P(r=H | s=H) = 1 - \theta_1\)，\(P(r=H | s=T) = \theta_2\)。

因此，决策规则为：推测 \(s=H\) 当且仅当
\[
(1 - \theta_1) \alpha > \theta_2 (1 - \alpha)
\]
等价地，可写为：
\[
\frac{\alpha}{1 - \alpha} > \frac{\theta_2}{1 - \theta_1}
\]
否则推测 \(s=T\)。

### (b) \(\theta_1 = \theta_2\) 时的直观解释

当 \(\theta_1 = \theta_2 = \theta\) 时，决策规则简化为：推测 \(s=H\) 当且仅当
\[
\alpha > \theta
\]
直观上，\(\alpha\) 是硬币正面的先验概率，\(\theta\) 是朋友报告错误的概率。如果硬币正面的概率 \(\alpha\) 大于朋友犯错的概率 \(\theta\)，则当朋友报告正面时，我们更相信硬币确实为正面，因为错误报告的可能性较低。反之，如果 \(\alpha \leq \theta\)，则朋友犯错的概率较高，报告正面可能源于实际为反面时的错误报告，因此推测反面更为合理。该规则体现了先验概率与朋友可靠性之间的权衡。

### (c) 朋友报告 \(n\) 次后的最小错误概率决策规则

朋友报告 \(n\) 次结果，记报告序列为 \(R = (r_1, r_2, \ldots, r_n)\)，其中每个 \(r_i\) 为 \(H\) 或 \(T\)。给定真实结果 \(s\)，多次报告条件独立。决策规则基于后验概率比：推测 \(s=H\) 当且仅当
\[
\frac{P(R | s=H) \alpha}{P(R | s=T) (1 - \alpha)} > 1
\]
即
\[
\frac{P(R | s=H)}{P(R | s=T)} > \frac{1 - \alpha}{\alpha}
\]
其中似然函数为：
\[
P(R | s=H) = \prod_{i=1}^n P(r_i | s=H) = (1 - \theta_1)^k \theta_1^{n-k}
\]
\[
P(R | s=T) = \prod_{i=1}^n P(r_i | s=T) = \theta_2^k (1 - \theta_2)^{n-k}
\]
这里 \(k\) 是报告序列中正面（\(H\)）的次数。因此，似然比为：
\[
\frac{P(R | s=H)}{P(R | s=T)} = \left( \frac{1 - \theta_1}{\theta_2} \right)^k \left( \frac{\theta_1}{1 - \theta_2} \right)^{n-k}
\]
决策规则为：推测 \(s=H\) 当且仅当
\[
\left( \frac{1 - \theta_1}{\theta_2} \right)^k \left( \frac{\theta_1}{1 - \theta_2} \right)^{n-k} > \frac{1 - \alpha}{\alpha}
\]
否则推测 \(s=T\)。

### (d) \(\theta_1 = \theta_2\) 且报告全正面时的直观解释

当 \(\theta_1 = \theta_2 = \theta\) 且报告序列全为正面（即 \(k = n\)）时，决策规则简化为：推测 \(s=H\) 当且仅当
\[
\left( \frac{1 - \theta}{\theta} \right)^n > \frac{1 - \alpha}{\alpha}
\]
直观上，\(\frac{1 - \theta}{\theta}\) 是朋友报告正确的似然比。如果朋友可靠（\(\theta < 0.5\)），则 \(\frac{1 - \theta}{\theta} > 1\)，不等式左边随 \(n\) 增大而指数增长。此时，即使先验概率 \(\alpha\) 较小，多次正面报告也会使后验概率强烈倾向于真实结果为正面，因为朋友错误报告的可能性低。反之，如果朋友不可靠（\(\theta > 0.5\)），则 \(\frac{1 - \theta}{\theta} < 1\)，左边随 \(n\) 增大而衰减，多次正面报告反而表明朋友可能频繁出错，真实结果更可能为反面。当 \(\theta = 0.5\) 时，朋友报告无信息量，决策完全取决于先验概率。规则体现了随着观察次数 \(n\) 增加，证据权重增强，先验的影响被似然所调整。