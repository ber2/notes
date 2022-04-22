This is an [[Experiment]] whose goal is to understand the behaviour of estimation errors when [[Data Sampling|Sampling]], following the estimation method described in [[Overlaps Sampling]].

## Experiment setting
The behaviour of the error rate depends only on three input factors: `a`, `b` and `c`. These correspond to the sizes of the two sets and their intersection, follogin the notation in [[Overlaps Sampling]].

When `c` is very small in comparison to `a` and `b`, we are likely not to identify enough elements in the overlap of two samples; this will be a main source of errors.

In order to model what would happen in the case of the segment overlap job, we have created a parameter space of triples `(a, b, c)` by using a logarithmic scale, with values ranging from 10 to 10^7.

A single sample using a `0.1` rate has been drawn for each parameter triple `(a, b, c)` from the models.

We have computed the relative error when approximating `c`.

## One-sided model
 ### Code
 
 ```python
import pymc3 as pm
import numpy as np
import arviz as az
 
def one_sided_sample(A: int, B: int, AB: int,
                     sampling_rate: float = 0.1,
                     draws: int = 200, tune: int = 20, cores: int = 4) -> az.InferenceData:
    p_A = AB / A
    p_B = AB / B
    
    sample_size = int(np.floor(sampling_rate * A))
    
    with pm.Model() as m:
        S = pm.Binomial("S", sample_size, p_A)
        I = pm.Deterministic("I", S * A / sample_size)
        
        eps_a = pm.Deterministic("eps_a", pm.math.abs_(I - AB))
        eps_r = pm.Deterministic("eps_r", pm.math.abs_((I - AB) / AB))

        trace = pm.sample(draws,
	                     tune=tune,
						 cores=cores,
						 return_inferencedata=True)
    
    return trace
```
 
 ### Results
 
![](one-sided-3d.png)
![](one-sided-a.png)
![](one-sided-b.png)
![](one-sided-c.png)

### Conclusions

We observe that the relative error quickly decreases as the size of `c` increases. This is both due to the fact that the relative error is a decreasing function on the size of `c`, but also that as `c` increases the lihelihood of sampling elements in `C` also increases.

## Two-sided model

### Code

```python
import pymc3 as pm
import numpy as np
import arviz as az

def two_sided_samples(A: int, B: int, AB: int,
                      sampling_rate_A: float = 0.1, sampling_rate_B: float = 0.1, 
                      draws: int = 200, tune: int = 20, chains: int = 4, cores: int = 4) -> az.InferenceData:
    
    p_A = AB / A
    p_B = AB / B
    
    sample_size_A = int(np.floor(sampling_rate_A * A))
    sample_size_B = int(np.floor(sampling_rate_B * B))
    
    with pm.Model() as m:

        ST = pm.Binomial("ST", min(sample_size_A, sample_size_B), p_A* p_B)

        I = pm.Deterministic("I", pm.math.sqrt(ST * A * B / min(sample_size_A, sample_size_B)))

        eps_a = pm.Deterministic("eps_a", pm.math.abs_(I - AB))
        eps_r = pm.Deterministic("eps_r", pm.math.abs_((I - AB) / AB))

        trace = pm.sample(draws, tune=tune, chains=chains, cores=cores, return_inferencedata=True)
        
    return trace
```

### Results
![](two-sided-3d.png)
![](two-sided-a.png)
![](two-sided-b.png)
![](two-sided-c.png)

### Conclusions
We observe that the relative error decreases as `c` increases but takes much longer to drop below an acceptable threshold. The behaviour is significantly worse than in the case of one-sided samples.

## Further questions

### What happens to the one-sided samples if we reduce sampling rates to `0.05` and `0.01`?

The relative error increases slightly but at a very slow rate. Hence, this could be a very good alternative when we are sampling from very large datasets.

### What happens to the two-sided samples if we increase `s_B` to `0.3`?

The relative error is still ill-behaved. This approach does not seem very promising.

### What happens to two-sided samples if we average over `10` samples at `0.1` rate?

The relative error drops. After all, this is due to the law of large numbers and is the main reason why Monte Carlo methods (averaging over repeated samples) are so effective.
However, the computational cost and code complexity would increase.