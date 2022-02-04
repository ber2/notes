Jensen's inequality is a statement about swapping the integral of a concave function and a distribution.

More precisely.

__Theorem__. Let $(\Omega, A, \mu)$ be a probability space. If $g: \Omega \rightarrow \mathbb{R}$ is $\mu$-integrable and $\varphi: \mathbb{R} \to \mathbb{R}$ is convex, then
$$
\varphi(\int_\Omega g d\mu) \leq \int_\Omega \varphi g d\mu.
$$

A reformulation:

__Theorem__. If $f$ is concave and $p$ is a probability distribution,
$$
f(\mathbb{E}_{p(t)} t) \geq \mathbb{E}_{p(t)} f(t).
$$

Notice how the sign of the inequality changes with the statement that $f$ is concave instead of convex.