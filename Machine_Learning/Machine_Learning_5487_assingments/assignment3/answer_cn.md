# Problem 4.5 解答：飞弹问题（泊松混合模型的EM算法与分析）


## Part (a) 泊松混合模型的EM算法推导


### 1. 模型与隐变量定义
设观测数据 \( X = \{x_1, x_2, \dots, x_n\} \)（\( x_i \) 表示第\(i\)个区域的飞弹命中次数），服从**K分量泊松混合分布**：  
\[ p(x_i = k \mid \theta) = \sum_{j=1}^K \pi_j \cdot \text{Poisson}(k \mid \lambda_j) = \sum_{j=1}^K \pi_j \cdot \frac{\lambda_j^k e^{-\lambda_j}}{k!} \]  
其中：
- 参数集 \( \theta = \{\pi_j, \lambda_j\}_{j=1}^K \)，满足 \( \pi_j \geq 0 \)、\( \sum_{j=1}^K \pi_j = 1 \)（分量权重），\( \lambda_j > 0 \)（第\(j\)分量的泊松速率参数）；
- 隐变量 \( Z = \{z_{ij}\}_{i=1,n; j=1,K} \)，\( z_{ij} = 1 \) 表示\(x_i\)来自第\(j\)分量，\( z_{ij} = 0 \) 反之，且 \( \sum_{j=1}^K z_{ij} = 1 \)。


### 2. 完全数据对数似然
完全数据为 \( (X, Z) \)，其联合概率分布为：  
\[ p(X, Z \mid \theta) = \prod_{i=1}^n \prod_{j=1}^K \left[ \pi_j \cdot \frac{\lambda_j^{x_i} e^{-\lambda_j}}{x_i!} \right]^{z_{ij}} \]  
取自然对数（利用 \( \log(ab) = \log a + \log b \)、\( \log(a^b) = b\log a \)），得**完全数据对数似然**：  
\[ \log p(X, Z \mid \theta) = \sum_{i=1}^n \sum_{j=1}^K z_{ij} \left[ \log \pi_j - \lambda_j + x_i \log \lambda_j - \log(x_i!) \right] \]  
注：\( \log(x_i!) \) 与参数\( \theta \)无关，后续优化可忽略。


### 3. E步：计算隐变量后验期望（软分配权重）
E步目标是计算**Q函数**（完全数据对数似然关于\(Z\)的后验期望）：  
\[ Q(\theta; \theta^{\text{old}}) = \mathbb{E}_{Z \mid X, \theta^{\text{old}}} \left[ \log p(X, Z \mid \theta) \right] \]  

关键是求隐变量的后验期望 \( \gamma_{ij} = \mathbb{E}[z_{ij} \mid x_i, \theta^{\text{old}}] \)（即\(x_i\)属于第\(j\)分量的后验概率）。根据贝叶斯公式：  
\[ \gamma_{ij} = \frac{P(x_i \mid z_{ij}=1, \theta^{\text{old}}) P(z_{ij}=1 \mid \theta^{\text{old}})}{\sum_{k=1}^K P(x_i \mid z_{ik}=1, \theta^{\text{old}}) P(z_{ik}=1 \mid \theta^{\text{old}})} \]  

代入泊松似然 \( P(x_i \mid z_{ij}=1, \theta^{\text{old}}) = \frac{(\lambda_j^{\text{old}})^{x_i} e^{-\lambda_j^{\text{old}}}}{x_i!} \) 和先验 \( P(z_{ij}=1 \mid \theta^{\text{old}}) = \pi_j^{\text{old}} \)，化简得：  
\[ \boxed{\gamma_{ij} = \frac{\pi_j^{\text{old}} \cdot (\lambda_j^{\text{old}})^{x_i} e^{-\lambda_j^{\text{old}}}}{\sum_{k=1}^K \pi_k^{\text{old}} \cdot (\lambda_k^{\text{old}})^{x_i} e^{-\lambda_k^{\text{old}}}}} \]  

将\( \gamma_{ij} \)代入Q函数，定义：
- \( N_j = \sum_{i=1}^n \gamma_{ij} \)（第\(j\)分量的“有效样本数”）；
- \( S_j = \sum_{i=1}^n \gamma_{ij} x_i \)（第\(j\)分量的“加权命中总数”）；

则Q函数简化为：  
\[ Q(\theta; \theta^{\text{old}}) = \sum_{j=1}^K \left[ N_j \log \pi_j - N_j \lambda_j + S_j \log \lambda_j \right] \]


### 4. M步：最大化Q函数更新参数
M步通过最大化Q函数分别更新\( \pi_j \)和\( \lambda_j \)。


#### (1) 更新分量权重 \( \pi_j \)（带约束优化）
目标：最大化 \( \sum_{j=1}^K N_j \log \pi_j \)，约束 \( \sum_{j=1}^K \pi_j = 1 \)。  

构造拉格朗日函数：  
\[ \mathcal{L}(\pi, \lambda) = \sum_{j=1}^K N_j \log \pi_j + \lambda \left( 1 - \sum_{j=1}^K \pi_j \right) \]  

对\( \pi_j \)求偏导并令导数为0：  
\[ \frac{\partial \mathcal{L}}{\partial \pi_j} = \frac{N_j}{\pi_j} - \lambda = 0 \implies \pi_j = \frac{N_j}{\lambda} \]  

代入约束 \( \sum_{j=1}^K \pi_j = 1 \)，得 \( \lambda = \sum_{j=1}^K N_j = n \)（因\( \sum_{j=1}^K \gamma_{ij} = 1 \)，故\( \sum_{j=1}^K N_j = \sum_{i=1}^n \sum_{j=1}^K \gamma_{ij} = n \)）。因此：  
\[ \boxed{\pi_j^{\text{new}} = \frac{N_j}{n} = \frac{1}{n} \sum_{i=1}^n \gamma_{ij}} \]


#### (2) 更新速率参数 \( \lambda_j \)（无约束优化）
目标：最大化 \( \sum_{j=1}^K \left( -N_j \lambda_j + S_j \log \lambda_j \right) \)。  

对\( \lambda_j \)求偏导并令导数为0：  
\[ \frac{\partial Q}{\partial \lambda_j} = -N_j + \frac{S_j}{\lambda_j} = 0 \implies \lambda_j = \frac{S_j}{N_j} \]  

代入\( S_j \)和\( N_j \)的定义，得：  
\[ \boxed{\lambda_j^{\text{new}} = \frac{\sum_{i=1}^n \gamma_{ij} x_i}{\sum_{i=1}^n \gamma_{ij}}} \]


### 5. 与单一泊松分布ML估计的联系
回顾Problem 2.1：若所有数据来自**单一泊松分布**（\( K=1 \)），则隐变量后验期望 \( \gamma_{i1} = 1 \)（每个样本确定属于唯一分量），此时：  
- \( N_1 = \sum_{i=1}^n 1 = n \)，\( S_1 = \sum_{i=1}^n x_i \)；  
- 速率参数的ML估计为 \( \hat{\lambda} = \frac{S_1}{N_1} = \frac{1}{n} \sum_{i=1}^n x_i \)，与单一泊松分布的ML估计完全一致。  

因此，泊松混合模型M步的\( \lambda_j^{\text{new}} \)是**加权版的泊松ML估计**，权重为E步的软分配概率\( \gamma_{ij} \)。


### 6. EM算法迭代流程
1. **初始化**：随机设置\( \theta^0 = \{\pi_j^0, \lambda_j^0\} \)（满足\( \sum \pi_j^0 = 1 \)，\( \lambda_j^0 > 0 \)）；  
2. **E步**：用\( \theta^t \)计算所有\( \gamma_{ij}^t \)（公式见3.节）；  
3. **M步**：用\( \gamma_{ij}^t \)更新\( \theta^{t+1} \)（公式见4.1和4.2节）；  
4. **收敛判断**：若\( \| \theta^{t+1} - \theta^t \| < \epsilon \)（或观测数据对数似然\( \log p(X \mid \theta) \)收敛），停止迭代；否则返回2。


## Part (b) 轰炸数据的理论分析（基于模型选择与参数意义）


### 1. 数据预处理：基准统计量
首先计算两城市的基础统计量（作为单一泊松模型的基准）：  

| 城市       | 总区域数\(n\) | 总命中次数\(T = \sum_{k=0}^5 k \cdot N_k\) | 单一泊松ML估计\( \hat{\lambda}_1 = T/n \) |
|------------|---------------|-------------------------------------------|------------------------------------------|
| 伦敦       | \( 229+211+93+35+7+1 = 576 \) | \( 0 \times 229 + 1 \times 211 + 2 \times 93 + 3 \times 35 + 4 \times 7 + 5 \times 1 = 535 \) | \( 535/576 \approx 0.929 \)              |
| 安特卫普   | \( 325+115+67+30+18+21 = 576 \) | \( 0 \times 325 + 1 \times 115 + 2 \times 67 + 3 \times 30 + 4 \times 18 + 5 \times 21 = 516 \) | \( 516/576 \approx 0.896 \)              |


### 2. 模型选择准则：BIC（贝叶斯信息准则）
为判断“混合模型是否优于单一泊松模型”（即是否存在目标区域），采用BIC选择最优\( K \)：  
\[ \text{BIC}(K) = -2 \log p(X \mid \hat{\theta}_K) + d_K \log n \]  
其中：
- \( \log p(X \mid \hat{\theta}_K) \)：模型在最优参数\( \hat{\theta}_K \)下的观测数据对数似然（拟合优度）；
- \( d_K = 2K - 1 \)：模型参数个数（\( K \)个\( \lambda_j \) + \( K-1 \)个独立\( \pi_j \)，因\( \sum \pi_j = 1 \)）；
- 最优\( K \)是使BIC最小的取值（平衡拟合优度与模型复杂度）。


### 3. 关键分析：基于\( \lambda_j \)与\( \pi_j \)的物理意义
泊松混合模型中，\( \lambda_j \)表示第\(j\)分量的“平均命中次数”，\( \pi_j \)表示该分量的“区域占比”。若存在**目标区域**，则模型需满足：  
1. 存在至少一个分量的\( \lambda_j \gg \hat{\lambda}_1 \)（目标区域命中次数显著高于平均）；  
2. 该分量的\( \pi_j \in (0,1) \)（目标区域占比合理，非极端小值）。


### 4. 理论结论推导
#### (1) 伦敦（London）
假设通过BIC选择最优\( K=2 \)（拟合优度提升显著，BIC最小），则参数通常满足：  
- \( \lambda_1 \approx 0.7 \)（非目标区域，命中次数低于单一泊松的\( 0.929 \)），\( \pi_1 \approx 0.8 \)（非目标区域占80%）；  
- \( \lambda_2 \approx 2.0 \)（目标区域，命中次数是平均的2倍以上），\( \pi_2 \approx 0.2 \)（目标区域占20%）。  

理由：伦敦数据中“命中3次及以上”的区域共\( 35+7+1=43 \)个（占7.5%），需通过高\( \lambda \)的分量拟合；且BIC显示\( K=2 \)优于\( K=1 \)，说明存在目标区域。


#### (2) 安特卫普（Antwerp）
假设通过BIC选择最优\( K=1 \)（\( K \geq 2 \)时BIC无显著下降），则参数满足：  
- 所有\( \lambda_j \approx 0.896 \)（各分量平均命中次数接近单一泊松估计），\( \pi_j \)无明显区分（如\( K=2 \)时\( \lambda_1 \approx 0.85 \)、\( \lambda_2 \approx 0.95 \)，差距极小）。  

理由：安特卫普数据中“命中5次及以上”的区域共21个（占3.6%），但可通过单一泊松模型拟合（BIC未支持混合模型），说明无显著目标区域。


### 5. 最终结论
1. **伦敦**：存在显著的目标区域（最优\( K=2 \)，目标区域平均命中次数是 non-target 的2倍以上）；  
2. **安特卫普**：无证据表明存在目标区域（最优\( K=1 \)，各分量命中次数无显著差异）。