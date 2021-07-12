## Week 2

The whole week revolves around the central topic of [[Latent Variable]] models and how to train them through [[Expectation Maximization]].

### Latent Variable models

The concept of [[Latent Variable]] is introduced and discussed. Next, it is put in practice by discussing a [[GMM]] for a [[Probabilistic Clustering]] problem.

In order to train this [[GMM]], a toy example of [[Expectation Maximization]] is introduced and discussed, before moving on cover the topic in general.

### Expectation Maximization algorithm

The [[Expectation Maximization]] algorithm is discussed in detail. [[Concave function]]s are discussed, together with [[Jensen's inequality]] and the [[Kullback-Leibler divergence]], which are used to prove the mathematical details in the algorithm: the [[Expectation Maximization#E-step]] and the [[Expectation Maximization#M-step]].

### Applications of Expectation Maximization

#### K-means as a GMM

The [[K-means]] clustering algorithm is discussed from the probabilistic point of view. It turns out it may be described as an [[Expectation Maximization]] of a [[GMM]] model provided:

- The Gaussians in the [[GMM]] all have identity covariance matrices; their means are the centroids for each of the clusters.
- The E-step is simplified by approximating $p(t_i \vert x_i, \theta)$ via delta functions.

This trick, where we simplify the [[Expectation Maximization#E-step]] by considering a restricted set of distributions is an example of [[Variational inference]].

#### Probabilistic PCA

It turns our that [[PCA 1]] can be understood from a probabilistic point of view, and it allows to interpolate missing data. This leads to [[PPCA|Probabilistic PCA]], which is introduced and discussed.
