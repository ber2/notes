This is a 2020 paper by [Dstillery](https://dstillery.com/), available here in [pdf](https://dstillery.com/wp-content/uploads/2021/05/2020-IDFree_IEEE_BigData.pdf).

## TL;DR

A novel approach to encoding URLs for contextual targeting purposes is introduced and discussed, using an adaptation of the [[Word2Vec]] technique.

The approach shows promise for a post-cookiepocalyptic approach to targeting, although it relies on two steps that do not have a clear answer in absence of third-party cookies:
- Being able to relate two RTB requests to the same user/browser/device.
- Having a reliable way to infer conversion attribution.

We should be able to implement this approach, at least for experiment purposes, with reasonably little effort.


## Main goal

If we place ourselves in a post-cookiepocalyptic world, we have to face the problem of making brand-specific targeting decisions based only on information related to the digital ad opportunity itself, without relying on historical user behaviour (we commonly refer to this as the __contextual approach__ to online advertising).

The top prediction signal in the ad opportunity is the URL where the ad will be displayed. This paper presents a novel way to learn feature representation of URLs by using an __embedding approach__.

This approach is then benchmarked against other more naive approaches are then discussed.


## Learning URL Embeddings

### Input data

This is gathered from observed pairs of URLs tied to RTB bid requests with the same browser ID. The [[ML]] problem is then to predict the second URL given the first one.

As a second-step, we need conversion labels to train a binary classifier.

Both of these steps require third-party cookies or a comparable solution; this is out of the paper's scope.

### Embedding architecture

This is an update on the [[Word2Vec]] architecture, which is widely used in [[NLP]]. The main update is the addition of a previous, learnable hashing step. This serves several purposes:
- Dimensionality reduction (by mapping URLs over to hash vectors with relatively few collisions)
- No need to maintain vocabulary dictionaries.
- Allows to obtain the embedding vectors of previously unseen URLs.
- Allows for online training on an evolving inventory dataset.

The trick for hashing is that instead of using one hash function with targets in a large space, several intermediate hashing functions are used, and their outputs are combined to obtain a learnable linear combination that produces the final embedding vector.

So, in addition to an initial input hash function with a range of size $N$, $k$ intermediate hash functions are used, each with a smaller range of size $M$. Hence, each URL is mapped to $k$ intermediate embedding vectors. By using $k$ learnable parameters, one can learn the best linear combination of these hash vectors to produce the final embedding vector.

More in detail, the embedding layer contains two subnetworks.

Compression subnetwork:
- It learns an embedding matrix $B$ of size $M \cdot d$, where $d$ is the embedding dimension (128) and $M$ is the size of the intermediate hash range (250k)
- A trainable matrix $P$ of importance parameters of size $N \cdot k$, where $k$ is the number of intermediate hash functions (2) and $N$ is the size of the input hash range (3.5M).

Decompression layer:
- A weight matrix of size $N \cdot d$.

### Binary classifiers

This paper only implements logistic regression (plus a gridsearch on regularization parameters).

The focus is clearly placed on representation learning, not model selection.


### Results

This approach outperforms the naive approach where a one-hot encoding is used on the URLs. And, as a matter of fact, it proves to be very resilient with datasets where there is very little data: URLs that are not frequent in the URL data, seldom seen in the training data still can contribute heavily as features in a classification problem.

However, comparisons against models where feature representation is tied to URL tokens (or keyword extraction of website contents) is deliberately left out. These approaches are regarded as complementary during the introduction.

Embedded URLs are interpretable in the usual word2vec terms: _similar_ URLs end up with close vectors. Given a URL, one can then look up the closest URLs to it in terms of cosine distance.