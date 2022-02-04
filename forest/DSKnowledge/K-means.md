---
aliases: [KMeans]
---

The __K-means algorithm__ is an algorithm for clustering. It is perhaps the simplest unsupervised [[ML|Machine Learning]] algorithm, but very often an excellent choice when working on a task with a fixed number of clusters.

Its steps are the following. Given:
- Data points $X = \{ x_i \}_{i = 1\ldots N} \subset \mathbb{R}^m$.
- A target number of clusters, $k$.

1. Choose random _centroids_ ${\mu_1, \ldots, \mu_k} \subset \mathbb{R}^m$.
2. Assign, to each $x_i \in X$, a centroid $c_i$ such that
$$
c_i = \arg \min_j \| x_i - \mu_j \|.
$$
3. Update the centroids by computing the center of mass of all points with the same label:
$$
\mu_j = \frac{1}{\# \{c_i = c_j \} } \sum_{c_i = c_j} x_i  
$$
4. Repeat steps 2 and 3 until convergence.


One is free to pick any metric $\| \cdot \|$; the most frequent choice is the Euclidean one.


### Implementations

[[scikit-learn]] has an excellent [implementation](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html#).