# 6.4 Coin Tossing Problem

In this problem, we study the classic probability scenario of coin tossing but with two variations.

First variation: The coin is non-uniform. Let \(s\) denote the outcome of the coin toss (which can be heads \(H\) or tails \(T\)), with:
\[p(s=H)=\alpha, \alpha \in[0,1] \tag{6.5}\]

Second variation: You cannot directly observe the outcome of the coin toss, but instead rely on a friend to report the outcome. Unfortunately, your friend is unreliable — when the actual outcome is tails, they sometimes report heads, and vice versa. Let \(r\) denote the friend's report, with:
\[p(r=T | s=H)=\theta_{1} \tag{6.6}\]
\[p(r=H | s=T)=\theta_{2} \tag{6.7}\]
where \(\theta_{1}, \theta_{2} \in[0,1]\). Your task is: given the friend's report, infer the true outcome of the coin toss.

## (a) Optimal Decision Function When Friend Reports Heads (\(r=H\))

When the friend reports heads (\(r=H\)), we want to determine the optimal decision rule that minimizes the probability of error. According to Bayesian decision theory, the optimal decision is to choose the state with the higher posterior probability given the observation.

Using Bayes' theorem, the posterior probabilities are proportional to the product of likelihood and prior:

\[P(s=H | r=H) \propto P(r=H | s=H) P(s=H) = (1 - \theta_1) \alpha\]
\[P(s=T | r=H) \propto P(r=H | s=T) P(s=T) = \theta_2 (1 - \alpha)\]

The optimal decision rule is: infer \(s=H\) if and only if
\[(1 - \theta_1) \alpha > \theta_2 (1 - \alpha)\]

This can be rewritten as:
\[\frac{\alpha}{1 - \alpha} > \frac{\theta_2}{1 - \theta_1}\]

Otherwise, infer \(s=T\).

## (b) Intuitive Explanation When \(\theta_1 = \theta_2\)

When \(\theta_1 = \theta_2 = \theta\), the decision rule simplifies to: infer \(s=H\) if and only if
\[\alpha > \theta\]

Intuitive explanation: 
- \(\alpha\) represents the prior probability that the coin shows heads
- \(\theta\) represents the probability that the friend gives an incorrect report

If the coin is more likely to show heads (\(\alpha\)) than the friend is to make an error (\(\theta\)), then when the friend reports heads, we should trust that the actual outcome is indeed heads. Conversely, if the friend's error probability is higher than the coin's bias toward heads, then a report of heads is more likely to be an error, and we should infer that the actual outcome was tails.

## (c) Minimum Error Probability Decision Rule with \(n\) Independent Reports

When the friend provides \(n\) independent reports of the same coin toss outcome, let \(R = (r_1, r_2, \ldots, r_n)\) denote the sequence of reports. The optimal decision rule minimizes the error probability by comparing posterior probabilities.

We infer \(s=H\) if and only if
\[\frac{P(R | s=H) \alpha}{P(R | s=T) (1 - \alpha)} > 1\]
which is equivalent to
\[\frac{P(R | s=H)}{P(R | s=T)} > \frac{1 - \alpha}{\alpha}\]

Given conditional independence of the reports:
\[P(R | s=H) = \prod_{i=1}^n P(r_i | s=H) = (1 - \theta_1)^k \theta_1^{n-k}\]
\[P(R | s=T) = \prod_{i=1}^n P(r_i | s=T) = \theta_2^k (1 - \theta_2)^{n-k}\]
where \(k\) is the number of heads reports in the sequence.

The likelihood ratio becomes:
\[\frac{P(R | s=H)}{P(R | s=T)} = \left( \frac{1 - \theta_1}{\theta_2} \right)^k \left( \frac{\theta_1}{1 - \theta_2} \right)^{n-k}\]

Thus, the decision rule is: infer \(s=H\) if and only if
\[\left( \frac{1 - \theta_1}{\theta_2} \right)^k \left( \frac{\theta_1}{1 - \theta_2} \right)^{n-k} > \frac{1 - \alpha}{\alpha}\]
Otherwise, infer \(s=T\).

## (d) Intuitive Explanation When \(\theta_1 = \theta_2\) and All Reports Are Heads

When \(\theta_1 = \theta_2 = \theta\) and all \(n\) reports are heads (\(k = n\)), the decision rule simplifies to: infer \(s=H\) if and only if
\[\left( \frac{1 - \theta}{\theta} \right)^n > \frac{1 - \alpha}{\alpha}\]

Intuitive explanation:
- The term \(\frac{1 - \theta}{\theta}\) represents the odds ratio of a correct report versus an incorrect report
- If the friend is reliable (\(\theta < 0.5\)), then \(\frac{1 - \theta}{\theta} > 1\), and the left side grows exponentially with \(n\)
- This means that multiple consistent reports from a reliable friend provide strong evidence for the reported outcome
- Even if the prior probability \(\alpha\) is low, enough consistent reports can overcome this prior belief
- Conversely, if the friend is unreliable (\(\theta > 0.5\)), then \(\frac{1 - \theta}{\theta} < 1\), and multiple heads reports actually provide evidence that the true outcome is tails
- The rule shows how accumulating evidence can strengthen or override our prior beliefs, depending on the reliability of the information source