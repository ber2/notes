What happens to the [[Mapping Rate]] when we use [[Sampling]] on one of the two sides (or both)?

On one side, we have the set $A$ of first party user ids. On the other side, we have the set $B$ of third party user ids. Thanks to the [[Mapper]], we have an [[Identity Graph]] encoding what we know at a point in time about which ids do in fact correspond to the same user. In other words, there is a correspondence
$$
M \subset A \times B
$$
defined as follows. Given $(\alpha, \beta) \in A \times B$, then $(\alpha, \beta) \in M$ if and only if $\alpha$ and $\beta$ belong to the same connected component of the identity graph.

__Definition__. We say that $\alpha \in A$ is _mappable_ if there exists a $\beta \in B$ such that $(\alpha, \beta) \in M$.

Obviously, the above definition is symmetric in $A$ and $B$.