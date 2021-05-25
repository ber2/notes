__The gamma distribution__ is given by the probability density function
$$
\Gamma(\gamma|a, b) = \frac{b^a}{\Gamma(a)} \gamma^{a-1}e^{-b\gamma},
$$
where the parameters $a, b$ and the variable $\gamma$ are all assumed to be positive.

The gamma function is a smooth extension of the factorial, and for positive integer $n$ we have $\Gamma(n) = (n-1)!$.

## Statistics:
- $\mathbb{E}[\gamma] = a/b$.
- $\mathrm{Mode}[\gamma] = (a-1)/b$.
- $\mathrm{Var}[\gamma] = a/b^2$.

## Examples

If you run 5km $\pm$ 100m a day, you could assume that the mean is $5$ and the variance is $0.1^2$.

By solving the two equations $a/b = 5$, $a/b^2 = 0.1^2$, it turns out that a Gamma distribution with parameters $a = 2500$, $b = 500$ actually models this problem.