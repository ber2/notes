---
aliases: [Soft Clustering]
---

__Probabilistic Clustering__, also known as __Soft Clustering__, is a type of unsupervised [[ML|Machine Learning]] problem that differs from usual [[Clustering]] by the fact that we replace attaching each data point to a cluster by computing the probability of attaching the data point to each cluster.

So, while in traditional clustering we want to model a function $f$ such that
$$
c = f(x)
$$
for given cluster classes $c$ and datapoints $x$, we now want to model
$$
p(c \vert x).
$$

One of the uses of probabilistic clustering is at picking hyperparameters, such as the number of clusters.

Finally, this approach also allows to synthesize new data points from the data.