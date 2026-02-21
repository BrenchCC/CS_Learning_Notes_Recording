# Problem 4.5 Solution: Missile Problem (EM Algorithm and Analysis for Poisson Mixture Model)

## Part (a) EM Algorithm Derivation for Poisson Mixture Model

### 1. Model and Latent Variable Definition
Let the observed data $X = \{x_1, x_2, \dots, x_n\}$ ($x_i$ is the number of missile hits in the $i$-th region) follow a **K-component Poisson mixture distribution**:  
$ p(x_i = k \mid \theta) = \sum_{j=1}^K \pi_j \cdot \text{Poisson}(k \mid \lambda_j) = \sum_{j=1}^K \pi_j \cdot \frac{\lambda_j^k e^{-\lambda_j}}{k!} $  
Where:
- Parameter set $\theta = \{\pi_j, \lambda_j\}_{j=1}^K$, with $\pi_j \geq 0$, $\sum_{j=1}^K \pi_j = 1$ (mixture weights), and $\lambda_j > 0$ (Poisson rate parameter for the $j$-th component);
- Latent variables $Z = \{z_{ij}\}_{i=1,n; j=1,K}$, where $z_{ij} = 1$ indicates $x_i$ comes from the $j$-th component, $z_{ij} = 0$ otherwise, and $\sum_{j=1}^K z_{ij} = 1$.

### 2. Complete Data Log-Likelihood
The complete data is $(X, Z)$, and its joint probability distribution is:  
$ p(X, Z \mid \theta) = \prod_{i=1}^n \prod_{j=1}^K \left[ \pi_j \cdot \frac{\lambda_j^{x_i} e^{-\lambda_j}}{x_i!} \right]^{z_{ij}} $  
Taking the natural logarithm (using $\log(ab) = \log a + \log b$, $\log(a^b) = b\log a$), we get the **complete data log-likelihood**:  
$ \log p(X, Z \mid \theta) = \sum_{i=1}^n \sum_{j=1}^K z_{ij} \left[ \log \pi_j - \lambda_j + x_i \log \lambda_j - \log(x_i!) \right] $  
Note: $\log(x_i!)$ is independent of parameters $\theta$ and can be ignored in optimization.

### 3. E-step: Compute Posterior Expectation of Latent Variables (Soft Assignment Weights)
The E-step aims to compute the **Q-function** (expected complete data log-likelihood given the current parameters):  
$ Q(\theta; \theta^{\text{old}}) = \mathbb{E}_{Z \mid X, \theta^{\text{old}}} \left[ \log p(X, Z \mid \theta) \right] $  

The key is to compute the posterior expectation of the latent variables $\gamma_{ij} = \mathbb{E}[z_{ij} \mid x_i, \theta^{\text{old}}]$ (i.e., the posterior probability that $x_i$ belongs to the $j$-th component). Using Bayes' rule:  
$ \gamma_{ij} = \frac{P(x_i \mid z_{ij}=1, \theta^{\text{old}}) P(z_{ij}=1 \mid \theta^{\text{old}})}{\sum_{k=1}^K P(x_i \mid z_{ik}=1, \theta^{\text{old}}) P(z_{ik}=1 \mid \theta^{\text{old}})} $  

Substituting the Poisson likelihood $P(x_i \mid z_{ij}=1, \theta^{\text{old}}) = \frac{(\lambda_j^{\text{old}})^{x_i} e^{-\lambda_j^{\text{old}}}}{x_i!}$ and prior $P(z_{ij}=1 \mid \theta^{\text{old}}) = \pi_j^{\text{old}}$, we simplify to:  
$ \boxed{\gamma_{ij} = \frac{\pi_j^{\text{old}} \cdot (\lambda_j^{\text{old}})^{x_i} e^{-\lambda_j^{\text{old}}}}{\sum_{k=1}^K \pi_k^{\text{old}} \cdot (\lambda_k^{\text{old}})^{x_i} e^{-\lambda_k^{\text{old}}}}} $  

Substituting $\gamma_{ij}$ into the Q-function, define:
- $N_j = \sum_{i=1}^n \gamma_{ij}$ (effective sample count for the $j$-th component);
- $S_j = \sum_{i=1}^n \gamma_{ij} x_i$ (weighted total hits for the $j$-th component);

Then the Q-function simplifies to:  
$ Q(\theta; \theta^{\text{old}}) = \sum_{j=1}^K \left[ N_j \log \pi_j - N_j \lambda_j + S_j \log \lambda_j \right] $

### 4. M-step: Maximize Q-function to Update Parameters
The M-step updates $\pi_j$ and $\lambda_j$ by maximizing the Q-function.

#### (1) Update Mixture Weights $\pi_j$ (Constrained Optimization)
Goal: Maximize $\sum_{j=1}^K N_j \log \pi_j$, subject to $\sum_{j=1}^K \pi_j = 1$.  

Construct the Lagrangian:  
$ \mathcal{L}(\pi, \lambda) = \sum_{j=1}^K N_j \log \pi_j + \lambda \left( 1 - \sum_{j=1}^K \pi_j \right) $  

Take partial derivative w.r.t. $\pi_j$ and set to zero:  
$ \frac{\partial \mathcal{L}}{\partial \pi_j} = \frac{N_j}{\pi_j} - \lambda = 0 \implies \pi_j = \frac{N_j}{\lambda} $  

Substitute into the constraint $\sum_{j=1}^K \pi_j = 1$, yielding $\lambda = \sum_{j=1}^K N_j = n$ (since $\sum_{j=1}^K \gamma_{ij} = 1$, we have $\sum_{j=1}^K N_j = \sum_{i=1}^n \sum_{j=1}^K \gamma_{ij} = n$). Thus:  
$ \boxed{\pi_j^{\text{new}} = \frac{N_j}{n} = \frac{1}{n} \sum_{i=1}^n \gamma_{ij}} $

#### (2) Update Rate Parameters $\lambda_j$ (Unconstrained Optimization)
Goal: Maximize $\sum_{j=1}^K \left( -N_j \lambda_j + S_j \log \lambda_j \right)$.  

Take partial derivative w.r.t. $\lambda_j$ and set to zero:  
$ \frac{\partial Q}{\partial \lambda_j} = -N_j + \frac{S_j}{\lambda_j} = 0 \implies \lambda_j = \frac{S_j}{N_j} $  

Substituting definitions of $S_j$ and $N_j$:  
$ \boxed{\lambda_j^{\text{new}} = \frac{\sum_{i=1}^n \gamma_{ij} x_i}{\sum_{i=1}^n \gamma_{ij}}} $

### 5. Connection to Single Poisson ML Estimate
Recall Problem 2.1: If all data comes from a **single Poisson distribution** ($K=1$), then the posterior expectation $\gamma_{i1} = 1$ (each sample definitively belongs to the single component). Then:
- $N_1 = \sum_{i=1}^n 1 = n$, $S_1 = \sum_{i=1}^n x_i$;
- The ML estimate for the rate parameter is $\hat{\lambda} = \frac{S_1}{N_1} = \frac{1}{n} \sum_{i=1}^n x_i$, identical to the single Poisson ML estimate.

Thus, the M-step update $\lambda_j^{\text{new}}$ is a **weighted version of the Poisson ML estimate**, with weights given by the soft assignment probabilities $\gamma_{ij}$ from the E-step.

### 6. EM Algorithm Iteration Process
1. **Initialization**: Randomly set $\theta^0 = \{\pi_j^0, \lambda_j^0\}$ (satisfying $\sum \pi_j^0 = 1$, $\lambda_j^0 > 0$);
2. **E-step**: Compute all $\gamma_{ij}^t$ using $\theta^t$ (formula in Section 3);
3. **M-step**: Update $\theta^{t+1}$ using $\gamma_{ij}^t$ (formulas in Sections 4.1 and 4.2);
4. **Convergence Check**: If $\| \theta^{t+1} - \theta^t \| < \epsilon$ (or the observed data log-likelihood $\log p(X \mid \theta)$ converges), stop; else return to Step 2.

## Part (b) Theoretical Analysis of Bombing Data (Based on Model Selection and Parameter Interpretation)

### 1. Data Preprocessing: Baseline Statistics
First, compute basic statistics for both cities (as benchmarks for single Poisson models):

| City       | Total Regions $n$ | Total Hits $T = \sum_{k=0}^5 k \cdot N_k$ | Single Poisson ML Estimate $\hat{\lambda}_1 = T/n$ |
|------------|-------------------|------------------------------------------|----------------------------------------------------|
| London     | $576$             | $535$                                    | $535/576 \approx 0.929$                           |
| Antwerp    | $576$             | $516$                                    | $516/576 \approx 0.896$                           |

### 2. Model Selection Criterion: BIC (Bayesian Information Criterion)
To determine whether a mixture model is better than a single Poisson model (i.e., whether target regions exist), use BIC to select the optimal $K$:  
$ \text{BIC}(K) = -2 \log p(X \mid \hat{\theta}_K) + d_K \log n $  
Where:
- $\log p(X \mid \hat{\theta}_K)$: log-likelihood of observed data under optimal parameters $\hat{\theta}_K$ (goodness-of-fit);
- $d_K = 2K - 1$: number of model parameters ($K$ $\lambda_j$'s + $K-1$ independent $\pi_j$'s, since $\sum \pi_j = 1$);
- The optimal $K$ minimizes BIC (balancing goodness-of-fit and model complexity).

### 3. Key Analysis: Physical Meaning of $\lambda_j$ and $\pi_j$
In the Poisson mixture model, $\lambda_j$ represents the "average hit count" for the $j$-th component, and $\pi_j$ represents the "proportion of regions" belonging to that component. If **target regions** exist, the model must satisfy:
1. At least one component has $\lambda_j \gg \hat{\lambda}_1$ (hit count significantly higher than average);
2. That component's $\pi_j \in (0,1)$ (reasonable proportion, not extremely small).

### 4. Theoretical Conclusion Derivation
#### (1) London
Assume BIC selects optimal $K=2$ (significant improvement in goodness-of-fit, minimal BIC). Then parameters typically satisfy:
- $\lambda_1 \approx 0.7$ (non-target regions, hit count below single Poisson's $0.929$), $\pi_1 \approx 0.8$ (80% non-target regions);
- $\lambda_2 \approx 2.0$ (target regions, hit count over twice the average), $\pi_2 \approx 0.2$ (20% target regions).

Reason: London data has $35+7+1=43$ regions with 3 or more hits (7.5%), requiring a high-$\lambda$ component for fitting; and BIC shows $K=2$ outperforms $K=1$, indicating target regions exist.

#### (2) Antwerp
Assume BIC selects optimal $K=1$ (BIC does not decrease significantly for $K \geq 2$). Then parameters satisfy:
- All $\lambda_j \approx 0.896$ (component hit counts close to single Poisson estimate), $\pi_j$ show no clear separation (e.g., for $K=2$, $\lambda_1 \approx 0.85$, $\lambda_2 \approx 0.95$, very small difference).

Reason: Antwerp data has 21 regions with 5 or more hits (3.6%), but can be fit by a single Poisson model (BIC does not support a mixture model), indicating no significant target regions.

### 5. Final Conclusion
1. **London**: Significant target regions exist (optimal $K=2$, target region hit count over twice that of non-target);
2. **Antwerp**: No evidence of target regions (optimal $K=1$, component hit counts show no significant difference).