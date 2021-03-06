## Week 1

### Introduction to Bayesian Methods

The first week of the course is an introduction to [[Bayesian]] statistics.

The [[Bayes Theorem]] on conditional probability is discussed and the main point is that a frequentist will see the data as an output of a fixed model while a [[Bayesian]] will think of the model as an output of the data.

In that sense, given data $X$ and a model which depends on weights $\theta = \left\{ \theta_1, \ldots, \theta_k \right\}$, a Bayesian is mostly interested in estimating
$$
\mathrm{p}(X | \theta),
$$
that is: the probability of observing $X$ given weights $\theta$.

Next, [[Bayesian Network]]s are discussed as a means to define a model using a causality graph. An example is discussed in detail.

Finally, [[Bayesian Linear Regression]] is discussed and a [[MLE]] estimation of the Gaussian mean is discussed. Assuming that:
1. The probability of the targets given the weights and features follows a normal distribution with diagonal deviation
2. The distribution of the weights follows a normal distribution centered at zero
the minimum is attained by minimizing the loss function of linear regression with L2-regularization.

Similarly, if we assume that the distributions are uniform, we attain L1-regularization.

### Conjugate priors

In the Bayes formula:
$$
p(\theta \vert X) = \frac{p(X \vert \theta) p(\theta)}{p(X)},
$$
modelling the distribution of the evidence, $p(X)$, is very hard. So we try to come up with ideas to optimize the posterior distribution without estimating the evidence.

One method to do so is the [[MAP|Maximum a posteriori]] principle.

Another method is to use [[Conjugate distributions]]. These are explained and a few examples are computed:
- [[Gamma distribution]], [[Normal distribution]] and [[Precision]].
- [[Bernoulli distribution]] and [[Beta distribution]].