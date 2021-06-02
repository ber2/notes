The __Beta distribution__ is given by the density function
$$
B(x \vert a, b) = \frac{1}{B(a, b)} x^{a - 1}(1 - x)^{b - 1}, \quad x \in [0, 1],\quad a, b > 0.
$$

It is very useful to model random variables which have finite support.

The normalization constant is given by the _beta function_, which can be expressed in terms of the gamma function:
$$
\frac{1}{B(a, b)} = \frac{\Gamma(a+b)}{\Gamma(a)\Gamma(b)}.
$$

## Statistics:
- $\mathbb{E}[x] = a/(a + b)$.
- $\mathrm{Mode}[x] = (a-1)/(a+b-2)$.
- $\mathrm{Var}[x] = ab/((a+b)^2(a+b-1))$.