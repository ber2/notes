__Thompson sampling__ is a [[Bayesian]] approach to the solving the [[Explore vs Exploit Dilemma]] in the context of [[MAB|Multi-Armed Bandit]]s.

The idea is the following: at each iteration, we draw a sample from the posterior distribution of all the observations so far and exploit the one giving the greatest sample value.

In most situations, the Bayesian inference involved in estimating the posterior distribution requires some sampling.

However, in the case of [[Bernoulli bandits]] (from counting data; these are pervasive in AdTech), we know that the the [[Bernoulli distribution]] is conjugate to a [[Beta distribution]] with parameters $a = 1 + S$, $b = 1 + F$, where $S$ and $F$ are, respectively, the number of successes and failures.
