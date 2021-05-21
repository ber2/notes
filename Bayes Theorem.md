Bayes Theorem could also be described as _the formula for conditional probability_.

__Theorem__. Given two different events $a, b$ with $\mathrm{p}(b) \neq 0$,
$$
\mathrm{p}(a \vert b) = \frac{\mathrm{p}(b \vert a)\mathrm{p}(a)}{\mathrm{p}(b)}.
$$

_Proof_. We have that, by definition of conditional probability:
$$
\mathrm{p}(a \vert b) = \frac{\mathrm{p}(a \cap b)}{\mathrm{p}(b)}.
$$
Now, from here we deduce that
$$
\mathrm{p}(a \cap b) = \mathrm{p}(a \vert b)\mathrm{p}(b) = \mathrm{p}(b \vert a)\mathrm{p}(a),
$$
and the formula in the statement follows.


All of the factors in the formula have names in the literature:
- $\mathrm{p}(a \vert b)$ is the __posterior probability of $a$ given $b$__.
- $\mathrm{p}(b \vert a)$ is the likelihood of $b$ given $a$
- $\mathrm{p}(a)$ and $\mathrm{p}(b)$ are the __prior probabilities__, given that they express the probability of $a$ or $b$ before observations.