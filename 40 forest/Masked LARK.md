__Masked LARK__ stands for __Masked Learning, Aggregation & Reporting Workflow__.

It is a proposal for a privacy-first approach to fraud detection, targeting and conversion reporting done by [[Microsoft Research]] in response to the planned deprecation of third-party cookies.

The main idea is to use a differentially private [[Map-Reduce]] in which user browsers take care of applying a secure _Map_ operation by creating keys and data for aggregation based on local user activity. Afterwards, helpers (decentralized servers) apply a differentially private _Reduce_ operation.

### References

- Talk slides presenting the project. [pdf](https://raw.githubusercontent.com/WICG/privacy-preserving-ads/main/presentations/20210726%20-%20Masked%20LARK%20presentation.pdf)