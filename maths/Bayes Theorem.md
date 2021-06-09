Bayes Theorem could also be described as _the formula for conditional probability_.

__Theorem__. Given two different events $a, b$ with $p(b) \neq 0$,
$$
p(a \vert b) = \frac{p(b \vert a)p(a)}{p(b)}.
$$

_Proof_. We have that, by definition of conditional probability:
$$
p(a \vert b) = \frac{p(a \cap b)}{p(b)}.
$$
Now, from here we deduce that
$$
p(a \cap b) = p(a \vert b)p(b) = p(b \vert a)p(a),
$$
and the formula in the statement follows.


All of the factors in the formula have names in the literature:
- $p(a \vert b)$ is the __posterior probability of $a$ given $b$__.
- $p(b \vert a)$ is the likelihood of $b$ given $a$
- $p(a)$ and $p(b)$ are the __prior probabilities__, given that they express the probability of $a$ or $b$ before observations.