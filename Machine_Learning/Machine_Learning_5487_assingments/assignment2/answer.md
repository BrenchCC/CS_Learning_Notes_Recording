### English Version

#### Problem 3.8 Bayesian estimation for a Bernoulli distribution

**(a) Likelihood function**

Given i.i.d. samples \(\mathcal{D} = \{x_1, \ldots, x_n\}\) from a Bernoulli distribution with parameter \(\pi\), the likelihood is:
\[
p(\mathcal{D} | \pi) = \prod_{i=1}^n p(x_i | \pi) = \prod_{i=1}^n \pi^{x_i} (1-\pi)^{1-x_i} = \pi^{\sum_{i=1}^n x_i} (1-\pi)^{n - \sum_{i=1}^n x_i} = \pi^s (1-\pi)^{n-s}
\]
where \(s = \sum_{i=1}^n x_i\) is the sufficient statistic.

**(b) Posterior distribution with uniform prior**

Assume a uniform prior \(p(\pi) = 1\) for \(0 \leq \pi \leq 1\). The posterior is proportional to the likelihood times the prior:
\[
p(\pi | \mathcal{D}) \propto p(\mathcal{D} | \pi) p(\pi) = \pi^s (1-\pi)^{n-s}
\]
The normalization constant is given by the integral:
\[
\int_0^1 \pi^s (1-\pi)^{n-s} d\pi = \frac{\Gamma(s+1)\Gamma(n-s+1)}{\Gamma(n+2)}
\]
using the identity provided. Thus, the posterior distribution is:
\[
p(\pi | \mathcal{D}) = \frac{\Gamma(n+2)}{\Gamma(s+1)\Gamma(n-s+1)} \pi^s (1-\pi)^{n-s}
\]
This is a Beta distribution with parameters \(\alpha = s+1\) and \(\beta = n-s+1\).

For \(n=1\), \(s\) can be 0 or 1:
- If \(s=0\), \(p(\pi | \mathcal{D}) = \frac{\Gamma(3)}{\Gamma(1)\Gamma(2)} \pi^0 (1-\pi)^1 = \frac{2!}{0! \cdot 1!} (1-\pi) = 2(1-\pi)\)
- If \(s=1\), \(p(\pi | \mathcal{D}) = \frac{\Gamma(3)}{\Gamma(2)\Gamma(1)} \pi^1 (1-\pi)^0 = \frac{2}{1 \cdot 1} \pi = 2\pi\)

The plots for \(n=1\):
- For \(s=0\): a linear decreasing function from 2 at \(\pi=0\) to 0 at \(\pi=1\).
- For \(s=1\): a linear increasing function from 0 at \(\pi=0\) to 2 at \(\pi=1\).

**(c) Predictive distribution**

The predictive distribution for a new sample \(x\) is:
\[
p(x | \mathcal{D}) = \int_0^1 p(x | \pi) p(\pi | \mathcal{D}) d\pi
\]
For \(x=1\):
\[
p(x=1 | \mathcal{D}) = \int_0^1 \pi \cdot p(\pi | \mathcal{D}) d\pi
\]
Since \(p(\pi | \mathcal{D})\) is Beta(\(s+1, n-s+1\)), the mean is:
\[
E[\pi | \mathcal{D}] = \frac{s+1}{(s+1) + (n-s+1)} = \frac{s+1}{n+2}
\]
Thus, \(p(x=1 | \mathcal{D}) = \frac{s+1}{n+2}\).

The effective Bayesian estimate of \(\pi\) is the posterior mean \(\frac{s+1}{n+2}\). This can be interpreted as adding two virtual samples, one success and one failure, to the ML estimate. This is known as Laplace smoothing or add-one smoothing.

**(d) ML and MAP estimates with uniform prior**

The ML estimate maximizes the likelihood \(p(\mathcal{D} | \pi)\):
\[
\hat{\pi}_{ML} = \frac{s}{n}
\]
With a uniform prior, the MAP estimate maximizes the posterior, which is proportional to the likelihood, so:
\[
\hat{\pi}_{MAP} = \frac{s}{n}
\]
The advantage of the Bayesian estimate \(\frac{s+1}{n+2}\) over the ML estimate is when \(n\) is small. For example, if \(n=1\) and \(s=0\), \(\hat{\pi}_{ML} = 0\), which predicts no success ever, but the Bayesian estimate \(\frac{1}{3}\) is more reasonable. The uniform prior adds virtual samples, making the estimate more robust to small sample sizes.

**(e) MAP estimates with alternative priors**

Consider priors \(p_1(\pi) = 2\pi\) and \(p_0(\pi) = 2(1-\pi)\) for \(0 \leq \pi \leq 1\).

For \(p_1(\pi) = 2\pi\):
The posterior is:
\[
p(\pi | \mathcal{D}) \propto p(\mathcal{D} | \pi) p_1(\pi) = \pi^s (1-\pi)^{n-s} \cdot 2\pi = 2 \pi^{s+1} (1-\pi)^{n-s}
\]
Maximizing the log-posterior:
\[
\frac{d}{d\pi} \left[ (s+1) \log \pi + (n-s) \log (1-\pi) \right] = \frac{s+1}{\pi} - \frac{n-s}{1-\pi} = 0
\]
Solving:
\[
\frac{s+1}{\pi} = \frac{n-s}{1-\pi} \implies (s+1)(1-\pi) = (n-s)\pi \implies s+1 = \pi(n+1) \implies \hat{\pi}_{MAP} = \frac{s+1}{n+1}
\]

For \(p_0(\pi) = 2(1-\pi)\):
The posterior is:
\[
p(\pi | \mathcal{D}) \propto \pi^s (1-\pi)^{n-s} \cdot 2(1-\pi) = 2 \pi^s (1-\pi)^{n-s+1}
\]
Maximizing the log-posterior:
\[
\frac{d}{d\pi} \left[ s \log \pi + (n-s+1) \log (1-\pi) \right] = \frac{s}{\pi} - \frac{n-s+1}{1-\pi} = 0
\]
Solving:
\[
\frac{s}{\pi} = \frac{n-s+1}{1-\pi} \implies s(1-\pi) = (n-s+1)\pi \implies s = \pi(n+1) \implies \hat{\pi}_{MAP} = \frac{s}{n+1}
\]

The effective Bayesian estimates:
- For \(p_1\), \(\hat{\pi}_{MAP} = \frac{s+1}{n+1}\) adds one virtual success.
- For \(p_0\), \(\hat{\pi}_{MAP} = \frac{s}{n+1}\) adds one virtual failure.