What happens to the [[Mapping Rate]] when we use [[Sampling]] on one of the two sides (or both)?


## Sampling the mapping rate

We will perform the following experiment on mapping rates:
- Lookback: one day (2021-04-07), one week (2021-04-01 to 2021-04-07) and one month (2021-03).
- Sampling rate: 0.01, 0.05, 0.1, 0.5, 1 (on both sides)
- Benchmark: 10 runs per value

Total: 750 runs

## How fast is sampling?

If we use `org.apache.spark.sql.DataFrame.sample`, this is quite slow unless executed on an already cached dataframe. For the experiments, we computed the tables of mappable events on either side, kept only the first-party and third-party ids and cached that.

By using left joins, we are able to recover non mappable events as those with null id on the other side.

It seems that `df.sample` parses through the whole dataset in order to decide which rows to keep.

It could pay to sample if the win is large on the other side. However, it would be even better to sample prior to looking over the whole dataset.


## How fast is the pullback?
