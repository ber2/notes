## Dates

__End of submissions__: July 30th, 2021.

## Dataset

We are given 19 distinct features. For each feature and each pair of features, a query with an aggregation has been run to get `count, sum(click), sum(sales)`. These values are then modified by adding some Gaussian noise into them. 

There are three datasets:
- A small sample of non-aggregated labeled data to allow offline model development
- A dataset for aggregated noisy queries on individual features
- A dataset for aggregated noisy queries on pairs of features

## Noise

$(\varepsilon, \delta)$-Differential Privacy has been used to add noise to the metrics for each feature and pair of features.

The parameters for the noise are the following.
- $\varepsilon = 5$
- $\delta = 10^{-10}$
- L1-sensitivity: $1$
- $\sigma = 17$

The chosen privacy budget, $\varepsilon = 5$, is spread across all of the 190 queries: __this could be used to our advantage provided we are capable of describing a Bayesian model__.

Similarly, the added noise is Gaussian; the value of $\sigma$ actually can be adjusted; this is something we could find out with Bayesian inference.

- A [blog](https://desfontain.es/privacy/gaussian-noise.html) with a series of posts on the matter. In particular, [this post](https://desfontain.es/privacy/more-useful-results-dp.html) about how to exploit data with noise coming from differential privacy.

- Original paper on (epsilon, delta)-privacy. [pdf](https://link.springer.com/content/pdf/10.1007/11761679_29.pdf)

## Target

Given data points $x_i$, compute the probability $p_i$ of having a positive label. Then, given actual targets $y_i \in \{ 0, 1 \}$, the validation metric is the log-loss:
$$
\mathcal{L} = - \sum_i \left( y_i \log p_i + (1 - y_i) \log(1 - p_i) \right)
$$


## Ideas

### Only use the small dataset

Scikit-learn & Tensorflow

### Use aggregation counts without noise

### Model noise