The __Kullback-Leibler divergence__ is a way to measure the difference between two probability distributions.

__Def__. Let $p$ and $q$ be two probability distributions. The __Kullback-Leibler divergence__ of $p$ and $q$ is the value:
$$
\mathrm{KL}(q \Vert p) = \int q(x) \log \frac{q(x)}{p(x)} dx.
$$

The log factor in the integral measures the point-wise difference between both distributions in log-space, while the other factor weighs in order to compute the expected value.

### Properties

1. $\mathrm{KL}(q \Vert p) \neq \mathrm{KL}(p \Vert q)$. In particular, it does not define a distance in the mathematical sense.
2. $\mathrm{KL}(q \Vert q) = 0$.

__Prop__. $\mathrm{KL}(q \Vert p) \geq 0$.
_Proof_.
$$
-\mathrm{KL}(q \Vert p) = \mathbb{E}_q (- \log \frac{q}{p}) = \mathbb{E}_q (\log \frac{p}{q}) \leq \log (\mathbb{E}_q \frac{p}{q}) = \log \int q(x) \frac{p(x)}{q(x)} dx = 0.
$$
Notice that the inequality follows from [[Jensen's inequality]] because $\log$ is a concave function.