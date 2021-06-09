Let $I \subseteq \mathbb{R}$ be an interval.

__Def__. A function $f: I \rightarrow \mathbb{R}$ is said to be __concave__ if for any $a, b \in I$ we have that
$$
f(\alpha a + (1 - \alpha)b) \geq \alpha f(a) + (1 - \alpha)f(b), \quad \forall 0 \leq \alpha \leq 1 
$$

By substitution, given weights $\{ \alpha_i \}_{i = 1,\ldots,k} \subset \mathbb{R}$ such that
$$
\begin{eqnarray}
&\sum_{i = 1}^k \alpha_i = 1, \\
&\alpha_i \geq 0 \quad \forall i=1,\ldots,k,
\end{eqnarray}
$$
we have that, given $x_1, \ldots, x_k \in I$,
$$
f(\sum_{i = 1}^k \alpha_i x_i) = \sum_{i=1}^k \alpha_i f(a_i).
$$