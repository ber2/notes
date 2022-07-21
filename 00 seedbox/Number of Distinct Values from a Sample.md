---
alias: [hotel problem, NDV]
---

This is the output of [RED-255](https://hybridtheory.atlassian.net/browse/RED-255).

## Summary
We discuss four estimation methods for the number of distinct values from a sample.

Taking the business perspective into account, the ease of implementation and the accuracy, we suggest to keep sampling at a 5% rate and to implement Good-Turing estimators for unique distinct value KPIs.

## The problem
We would like to estimate the number of distinct values on a column of a database table, in a big data context where it is prohibitively expensive to run the `COUNT DISTINCT` on the whole table.

This problem has two variations. If we are allowed to perform a full scan on the table, then it is possible to obtain a very good estimation of the number of distinct values using hashing methods with a constant memory requirement, using the [[HyperLogLog]] method. This is implemented in [[Spark]]'s `approx_count_distinct` SQL function.

We will focus on a much harder problem, in which we are not allowed to scan the whole table and we are given a small sample, containing a fraction of the total table rows. In this case, we will need to be far more pessimistic with our estimations.

This problem has a history in the literature, and some of the methods discussed have a motivation in a specific problem. For the sake of the discussion in here, we will fix some notation.

We are observing the values of a random discrete variable, with very high cardinality. Let $N$ be the total number of observations, out of which there are $U$ distinct values. We are given a sample of $m = \sigma N$ observations (in our case, $\sigma = 0.05$ or $0.1$).

- Problem: obtain an estimate $\widehat{U}$ for $U$.

## Methods
We will discuss four different estimation methods, and the trade-off between their accuracy and their difficulty of implementation.

### Naive approximation
The first approach that could be taken is to count the number of unique values $u$ appearing in the sample, and to scale in size by the sampling ratio $\sigma$. In order words, if $t = 1/\sigma$, then
$$
\widehat{U} = t u
$$
is our estimator.

**Pros**: this is the most straightforward estimate, can be implemented as a SQL operation.
**Cons**: the estimation is very poor

### Good-Turing
The Good-Turing frequency estimation was developed by Alan Turing and I. J. Good while breaking German ciphers at Bletchley Park during World War II. We use an adapted approach which is very common in [[NLP]] when estimating the size of the vocabulary of a corpus out of which only a portion of documents has been observed. A very common benchmark problem in the literature is to estimate the amount of words that were known by William Shakespeare, out of a sample of his works.

Let $u$ be the number of unique values observed. We will obtain our estimator by trying to compute the number of unseen values, $v$. Given our sample of $m$ elements, let $u_1$ be the number of values that appear exactly once on the sample. The probability of observing a previously unseen value if we add a new element into the sample is roughly $u_1 / m$. If we (naively) assume that this probability will not change as we keep adding elements into our sample, then we can estimate
$$
\widehat{v} = \frac{u_1}{m}(N - m),
$$
so we obtain:
$$
\widehat{U} = u + \widehat{v} = u + \frac{u_1}{m}(N - m) = u + (t - 1)u_1
$$

### Poisson
This approach is suggested in the [[Estimating the Prediction Function and the Number of unseen Species in Sampling with Replacement]] paper, which is formulated in ecological terms.

The number of appearances of each distinct value in the sample follows a Poisson distribution. With the notations of the previous section, we will also focus on estimating the value $v$ of previously unseen values.

The idea is to approximate the appearance of new values by using an estimated Poisson distribution learned from the sample values. Let $u_k$ be the number of distinct values appearing exactly $k$ times in the sample, for $k=1,\ldots,k_\max$. Then, we can estimate:
$$
\widehat{v} = \sum_{k=1}^{k_\max} u_k e^{-k} - \sum_{k=1}^{k_\max} u_k e^{-k(1 + t)} = \sum_{k=1}^{k_\max} u_k e^{-k} (1 - e^{-kt}),
$$
so
$$
\widehat{U} = u + \sum_{k=1}^{k_\max} u_k e^{-k} (1 - e^{-kt}).
$$

### Learned NDV Estimator
This approach is introduced in the [[estndv|learned NDV estimator]] paper, and implemented in the [[estndv]] Python package.

The paper authors propose an approach in which learning an estimator from a random sample is treated as a [[Supervised Task]], and they provide a trained [[Neural Network]] for the purpose, which is what is made available in the [[estndv]] package.

This model can be treated as a black box. It provides two methods.
- `sample_predict`: given a list containing the sample and the value $N$, it outputs a value for $\widehat{v}$.
- `profile_predict`: given the sample profile (ie: the list `[u_k for k in range(1, k_max + 1)]`), and the value $N$, it outputs a value for $\widehat{v}$.

## Comparison
We have performed an experiment on some of our available data. As a population, we have sampled the Sharethis events in EMEA for dates between 2022-06-13 and 2022-06-20 (both included).

We have extracted a count of unique users per country on the real data. Similarly, we have extracted two samples, both supplied as a sample profile in a CSV file:
- A sample with 5% of the total events ($t=20$)
- A sample with 10% of the total events ($t=10$)

We have applied each of the corresponding methods, in order to generate pairs `(country, estimation)`, that could then be compared against the reference `(country, truth)` values. In each case we have computed the absolute and relative errors. Allow us to proceed to visually compare the different methods.

First, by scattering true values vs estimations, we are able to inspect how each estimator performs by checking how far its points lie from the diagonal.

![[estimator_comparison_t20.png]]

![[estimator_comparison_t10.png]]

All estimations improve in general when $t=10$, which is of course expected. However, two facts are striking:

- The Poisson estimator performs very poorly in both cases.
- The estndv estimator performs very well on the larger sample. It is also the best estimator on the smaller sample, provided we focus on values which are large enough (average relative error: 0.14 for countries having at least 2M unique users).

These are the only behaviours that really change when the sample size changes. In the sequel, we will focus on the small sample, with $t=20$.

We can dive deeper into how each method performs by looking at scatters of true values against relative errors.

![[rel_error_scatter_t20.png]]

By zooming in away from small true values, we can tell that relative error estabilizes in each case.

![[rel_error_scatter_t20_zoom.png]]

In particular:
- Poisson estimations remain with an error consistently between $0.85$ and $0.92$. We conclude that this estimator is consistently bad and it underestimates by a lot. We will discard it from further analysis. According to the paper authors, this estimator becomes good when the sample size increases and they study detailed examples with $t \leq 2$, which implies observing half the available data in the sample; this is prohibitive in our case.
- Estndv estimations consistenly have errors below $0.5$.
- The errors for Good-Turing and Naive estimators seem to be quite correlated, with the former consistenly performing around 30% better than the other.

The above discussions are perhaps better reflected in the following plot comparing the distributions of relative errors.

![[error_dist_t20.png]]

Finally, let us also look at the distributions of unique user counts, while also plotting the true values.

![[estimation_dist_t20.png]]

## Conclusions
The error rates of naive estimations are very high. We should amend our estimations by implementing Good-Turing. While this estimation is considerably more involved than the naive one, it does not require anything but SQL operations on the data. This implies that we could easily adopt this estimation method in each of our use cases (spark on scala, pyspark, sparksql, redshift).

Whenever we need accurate estimations, we could rely on the estimations provided by Estndv, which are only available in Python.

The problem does not seem to improve when the sample size is larger, except for the fact that estimations become generally better.

## References
- [[estndv]]
- [[Estimating the Prediction Function and the Number of unseen Species in Sampling with Replacement]]
- [[Sampling-Based Estimation of the Number of Distinct Values of an Attribute]]

- Cross validated post: [Estimate Unique Number of Visitors](https://stats.stackexchange.com/questions/481166/estimate-unique-number-of-visitors)
- Cross validated post: [How can I estimate unique occurrence counts from a  random sampling of data?](https://stats.stackexchange.com/questions/19014/how-can-i-estimate-unique-occurrence-counts-from-a-random-sampling-of-data)