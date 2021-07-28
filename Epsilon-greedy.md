The __Epsilon-greedy__ algorithm is one of the simplest approaches to solving the [[Explore vs Exploit Dilemma]], in the context of [[MAB|Multi-Armed Bandit]]s.

The names comes from the choice of a hyperparameter $\varepsilon \in (0, 1)$, which stands for the probability of exploration. At each request, a value $r$ is sampled with uniform distribution from the $[0, 1]$ interval. If $r < \varepsilon$, we explore and pull an arm picked randomly. Otherwise, we pull the arm with maximum reward.
