This document is about the statistics of [[Data Sampling]] in the case of [[Segment Overlaps]], although the results can be used in more general intersection problems.

## Introduction

In this document we outline a proposal that aims at estimating the overlap between two segments by looking only at a small subset of the available data.

By using uniform sampling and [[Bayesian]] inference, we show that it is possible to discard more than 90% of events while having very good control of estimation errors.

The mathematical problem is about estimating the number of elements in the intersection of two sets by sampling them. As such, two very good examples of situations where we encounter this problem are:

- Computing the overlap between two segments.
- Computing the [[Mapping Rate]] between first party and third party users.

Although these situations have a different nature they have the following in common: we often find ourselves computing very large and costly SQL operations for reporting purposes. Then, aggregating on the resulting data takes many resources and does not scale well.

## Summary of results

### If you only need to sample on one side

In this case, you should always sample on the biggest set.
With the notation below, approximate the number of elements in the intersection by

$$
\frac{x  a}{S},
$$

where $x$ is the number of elements from the sample observed in the other set, $a$ is the number of elements in the bigger set and $S$ is the total number of elements in the sample. 

### If you need to sample on both sides

In such case, approximate the number of elements in the intersection by

$$
\sqrt{\frac{y  a  b}{T}},
$$

where $y$ is the number of elements observed simultaneously in both samples, $a$ and $b$ are the number of elements in both sets and $T$ is the number of elements in the smallest of the two samples.

Sampling is bad in the case in which the two sets are _nearly disjoint_, that is: the intersection of the two sets is very small compared to the sizes of both sets. In this case, it becomes very hard to find common elements as we sample, especially when we sample on both sides.

### Recommendation for the overlap problem

We recommend an approach which consists of sampling on both sides at a 10% rate, but only in those segments having a number of users above a certain threshold.

## Problem setting

Let $A$ and $B$ be two finite sets. We are interested in estimating the number of elements in the intersection of $A$ and $B$ from looking at a sample of elements from $A$ and/or $B$.

In this problem we assume that the sizes of $A$ and $B$ are known. In computational terms, we expect that the following operations are _cheap_:

- Counting the total number of elements in either set
- Checking whether an element of one set belongs to the other set.

__Notation__. We will denote $C = A \cap B$, and we will use lowercase letters $a = \#A$, $b = \#B$, $c = \#C$ to denote the cardinality of each set.

__Problem__. Estimate $c$ from sampling on $A$ and/or $B$.

## One-sided sampling

The statistical model associated to sampling only on one of the two sets is simpler and leads to much better error control.

We will assume that $a > b$ and sample only on $A$; this is both the case where we have better error control and performance gain.

__Definition__. __Sampling on $A$ with sampling rate $s$__, with $0 \leq s \leq 1$, is the action of taking $S = \lfloor sa \rfloor$ random elements from $A$ with uniform probability.

More precisely, all elements of $A$ have the same probability of being selected, $1 / a$. Therefore, the probability of picking an element in $C$ is $p = c / a$.

The random variable consisting of randomly selecting an element of $A$ and labelling it with $1$ if it belongs to $C$ and $0$ otherwise follows a Bernoulli distribution $Bern(p)$.

The random variable $X$ that counts the number of elements from a sample that fall in $C$ is equal to the sum of $S$ values sampled from $Bern(p)$.
Since the sum of independent Bernoulli distributions follows a Binomial distribution, we have:

$$
X \sim Binom(S, p).
$$

The expected value of such a distribution is $Sp$. So, if we use this expected value for an estimation, when $x$ is a value sampled from $X$, we have:

$$
x \simeq Sp = \frac{S c}{a}.
$$

From here, we have an estimate for $c$:

$$
c \simeq \frac{xa}{S}.
$$

__Example__. If $a = 200$, $b = 100$ and $c = 50$, if we sample with $s = 0.1$ on $A$, then $S = 20$ and $p = 0.25$. The expected value for $X$ is $x = S p = 5$, from which we would recover $c \simeq x a / S = 50$.

__Example__. From our estimation of $c$, we deduce estimates for the ratio of overlaps:

$$
\frac{c}{a} \simeq \frac{x}{S}, \quad
\frac{c}{b} \simeq \frac{xa}{Sb}.
$$

Since obtaining estimates for the overlap ratios is immediate once we have estimated $c$, we will focus on the latter and leave the former as an exercise for the reader.

## Two-sided sampling

When we sample on both sides, the complexity of the model increases. Suppose that we sample $A$ and $B$ with rates $s_A$ and $s_B$, respectively. The sample sizes will then be $S_A = \lfloor a s_A \rfloor$ and $S_B = \lfloor b s_B \rfloor$.

The probability of observing one of the $S_A$ elements from $A$ in $C$ is $p_A = c / a$. Similarly, $p_B = c / a$.

We will model the probability of seeing an element in common in both samples as the product of two independent Bernoulli distributions, which is again a Bernoulli distribution:

$$
Bern(p_A) \cdot Bern(p_B) \sim Bern(p_A \cdot p_B)
$$

In other words, the probability of picking one element simultaneously in both samples is $p_A  p_B$. If we denote $T = \min(S_A, S_B)$ for the size of the smallest sample, the count $Y$ of all elements falling in the intersection of both samples follows a Binomial distribution:

$$
Y \sim Binom(T, p_A p_B)
$$

So, if $y$ is sampled from $Y$, we have an expected value

$$
y \simeq T p_A p_B = \frac{T c^2}{ab},
$$

from where:

$$
c \simeq \sqrt{\frac{y a b}{T}}
$$

__Example__. Suppose that $a = 400$, $b = 200$ and $c = 100$. We have $p_A = 0.25$ and $p_B = 0.5$. By letting $s_A = s_B = 0.1$ we have $S_A = 40$ and $S_B = T = 20$. The expected value for $Y$ is $y = T * p_A * p_B = 2.5$. From here, we estimate:

$$
c \simeq \sqrt{\frac{2.5 \cdot 400 \cdot 200}{20}} = \sqrt{10000} = 100
$$

## Experiments

We conducted an experiment consisting about sampling in the setting of the [[Segment Overlaps]] problem:

- [[Experiment - Overlap Sampling errors]]
