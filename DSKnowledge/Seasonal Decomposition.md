
# Additive Time Series Decomposition
__Additive Time Series Decomposition__ is an algorithm for [[Time Series analysis]] which decomposes a time series $Y(t)$$ into a sum

$$
Y(t) = T(t) + S(t) + \varepsilon(t),
$$

where:
- $T(t)$, the [[Trend]], is obtained by applying a [[convolution filter]] to the data.
- $S(t)$, the [[Seasonal Component]], is the average of $Y(t) - T(t)$.
- $\varepsilon(t)$ is the error term.

# Multiplicative Time Series Decomposition

__Multiplicative Time Series Decomposition__ works exactly like [[Seasonal Decomposition#Additive Time Series Decomposition]], except for the fact that $Y(t)$ is decomposed into a product:

$$
Y(t) = T(t) \cdot S(t) \cdot \varepsilon(t)
$$

# Implementations

Both algorithms are available in the [[statsmodels]] [[Python]] packages, under `statsmodels.tsa.seasonal.seasonal_decompose`.

- [Docs](https://www.statsmodels.org/stable/generated/statsmodels.tsa.seasonal.seasonal_decompose.html).