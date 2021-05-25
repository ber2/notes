---
aliases: [Maximum a posteriori]
---

The __Maximum a posteriori__ is a method in [[Bayesian]] inference that attempts to estimate the posterior distribution by finding the parameters that maximize it.

In other words, in the Bayes formula:
$$
p(\theta \vert X) = \frac{p(X \vert \theta) p(\theta)}{p(X)},
$$
we try to find the value
$$
\theta_{\mathrm{MP}} = \mathrm{argmax}_\theta p(\theta \vert X).
$$

By substitution, and since the distribution of the evidence, $p(X)$, does not depend on $\theta$, we have
$$
\theta_{\mathrm{MP}} = \mathrm{argmax}_\theta p(X \vert \theta) p(\theta).
$$

This is an optimization problem that can be solved efficiently using numerical methods.

## More generally

The MAP is the solution to the optimization problem that computes $\min_\theta \mathcal{L}(\theta)$, where: 
$$
\mathcal{L}(\theta) = \mathbb{I}[\theta \neq \theta^*],
$$
ie: the indicator function that you do not end up with the true, ideal parameter value.

Other objective functions possible:
- $\mathcal{L}(\theta) = \mathbb{E}(\theta - \theta^*)^2$ produces the mean of the posterior.
- $\mathcal{L}(\theta) = \mathbb{E}|\theta - \theta^*|$ produces the median of the posterior.

### Problems

- MAP is not invariant to reparametrization; if we update our model, the position of the maximum of the distribution can change.

- MAP cannot be used as a prior for a following estimation step. In the formula:
$$
p_k(\theta) = \frac{p(x_k \vert \theta) p_{k-1}(\theta)}{p(x_k)},
$$
supplying $\theta_{\mathrm{MP}}$ as a prior results in
$$
p_k(\theta) = \delta(\theta - \theta_{\mathrm{MP}}),
$$
which does not bring any new information into the picture.

- Estimating the maximum may not be a very meaningful descriptor of the posterior distribution, as it may be isolated without much probability density around it.

- MAP does not allow to compute credible regions.