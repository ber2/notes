The __Precision__ $\gamma$ of a distribution is the inverse to its __Variance__.

For example, for $\mathcal{N}(\mu, \sigma^2)$, we have that
$$
\gamma = \frac{1}{\sigma^2}.
$$

Hence, describing the density function
$$
\mathcal{N}(\mu, \sigma^2) = \frac{1}{\sqrt{2\pi\sigma^2}}e^{- \frac{(x - \mu)^2}{2\sigma^2}}
$$
in terms of the precision, we obtain:
$$
\mathcal{N}(\mu, \gamma^{-1}) = \frac{\sqrt{\gamma}}{\sqrt{2\pi}}e^{- \gamma \frac{(x - \mu)^2}{2}}.
$$
