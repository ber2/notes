__Conjugate distributions__ is a method to avoid estimating the contribution of the evidence $p(X)$ in the Bayes formula.

In the Bayes formula,
$$
p(\theta \vert X) = \frac{p(X \vert \theta) p(\theta)}{p(X)},
$$
the likelihood $p(X \vert \theta)$ is fixed by the model, the prior $p(\theta)$ is our choice and the evidence $p(X)$ is fixed by data.

The idea is to select $\theta$ in such a way that it becomes easy to compute the posterior.

__Definition__. The prior and the posterior distributions are said to be _conjugate_ if they both lie in the same family of distributions.


## Examples

- The normal distribution is conjugate to itself with respect to itself. In other words, if the likelihood is normal, one can assume that both prior and posterior follow normal distributions. This is because then the product of likelihood and prior is now the product of two normal distributions, which is again a normal distribution.

- The [[Gamma distribution]] is conjugate to the [[Normal distribution]] with respect to the [[Precision]]. If, as a prior,
$$
p(\gamma) = \Gamma(\gamma \vert a, b),
$$
we have that the posterior $p(\gamma \vert x)$ is a scalar multiple of
$$
(\gamma^{1/2}e^{-\gamma\frac{(x-\mu)^2}{2}})(\gamma^{a-1}e^{-b\gamma}),
$$
which in turn is a scalar multiple of
$$
\gamma^{\frac{1}{2} + a - 1} e^{-\gamma(b + \frac{(x - \mu)^2}{2})}.
$$
Hence,
$$
p(\gamma \vert x) = \Gamma(a + 1/2, b + (x-\mu)^2/2)
$$

- The [[Beta distribution]] is conjugate to itself with respecto to the [[Bernoulli distribution]]. If
$$
p(X \vert \theta) = \theta^{N_1}(1 - \theta)^{N_0}
$$
and
$$
p(\theta) = B(\theta \vert a, b),
$$
we then have that
$$
p(\theta \vert X) = B(N_1 + a, N_0 + b)
$$

## Pros

- With this method we obtain an exact posterior.
- Easy for on-line learning.

## Cons

- For some models, the conjugate prior may be inadequate.