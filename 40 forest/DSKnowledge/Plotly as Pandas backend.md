This is related to modifying the default [[Pandas Global Settings]].

By default, [[Pandas]] defaults to [[Matplotlib]] when rendering data visualizations with `pandas.DataFrame.plot`. However, it is possible to modify the plotting backend to [[Plotly]].

The quid is the following, assuming [[Jupyter#Lab]] and [[Plotly]] are installed and configured properly to work with each other.

```python
import pandas as pd

pd.set_option("plotting.backend", "plotly")
```

More details about configuration and the plots that become easily accessible in the reference.

- _How to use Plotly as Pandas Plotting Backed_. [post](https://towardsdev.com/how-to-use-plotly-as-pandas-plotting-backend-123ff5378003)