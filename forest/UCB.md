---
aliases: [Upper Confidence Bound]
---

The __Upper Confidence Bound__ algorithm is used as a method to solve the [[Explore vs Exploit Dilemma]] in the context of [[MAB|Multi-Armed Bandit]]s.

The aim is to construct a confidence interval of each arm's true performance. Then, we select the arm with the highest upper bound for the confidence interval.

Given a sample of $N$ observations with mean $\mu$, the [[Chernoff-Hoeffding bound]] says that:
$$
P(\widehat{\mu} > \mu + \varepsilon) \leq \exp(-2\varepsilon^2 N), \quad \varepsilon > 0,
$$
where $\widehat{\mu}$ is the true mean (in the frequentist sense).

Given arms $A_j, j = 1, \ldots, K$, we pick bounds $\varepsilon_j$ so that $\widehat{\mu}_j < \mu_j + \varepsilon_j$. By using the above bound, and setting a threshold of $N^4$ we can select
$$
\varepsilon_j = \sqrt{-\frac{2\logN}{N_j}},
$$
where $N$ is the number of observations and $N_j$ is the number of times that arm $A_j$ was pulled.