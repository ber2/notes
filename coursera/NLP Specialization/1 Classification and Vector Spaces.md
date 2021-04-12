This course is mostly a reminder and gentle introduction to NLP, centered around the problem of
sentiment analysis.


## Week 1. Logistic regression

Several tools for feature extraction on a given text corpus are introduced:

1) Elimination of special characters

2) [[Tokenization]] into words

3) [[Stopwords]] removal

4) Stemming

5) Converting to lowercase

Furthermore, in order to be able to treat multiple sentences at once:

- [[Zero-Padding]]

- Removing punctuation and special characters

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/1-classification-vector-spaces/week1-logistic-regression.ipynb)

The problem is tweet [[Sentiment Analysis]].

It is attacked via an ad-hoc implementation of [[logistic regression]] with [[gradient descent]].

Features are encoded using a [[bag-of-words]] approach.


## Week 2. Naive Bayes

This week revolves around [[Naive Bayes]] models. The Naive Bayes model is derived from Bayes' theorem
on conditional probability under the (naive) assumption that features are independent from one
another. While this is almost never true, the predictions obtained from such a model are already
good enough in some cases and serve as a very powerful baseline to beat by more involved models.

Other topics discussed:

1) [[Laplacian smoothing]]: adding epsilons to the frequency counts to avoid underflow problems.

2) [[Log-likelihood]]: taking logarithms of probabilities so that we do not underflow by multiplying a
bunch of probabilities (which are always < 1).

### Applications of Naive Bayes

- Author identification
- Spam filtering
- Information retrieval
- Word disambiguation

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/1-classification-vector-spaces/week2-naive-bayes.ipynb)

The problem is, again, tweet [[Sentiment Analysis]].

It is attacked via an ad hoc [[Naive Bayes]] model.


## Week 3. Vector spaces

Some simple vectorization methods are introduced and discussed. This includes a discussion of linear
algebra on numpy and some concepts used in word embeddings such as [[cosine similarity]].

As a visualization technique, [[PCA]] is introduced and discussed. This includes a [demonstration
notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/1-classification-vector-spaces/week3-pca-demo.ipynb) using [[scikit-learn]]'s PCA implementation.

### Programming Assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/1-classification-vector-spaces/week3-vector-spaces.ipynb)

The problem is to predict the countries given their capital cities.

Revolves around the manipulation of [[word vectors]], the usage of [[PCA]] in order to project into plots
and the comparison of word vectors using a similarity measure.


## Week 4. Machine translation and document search

After having introduced word vectors in the previous week, this week is spent introducing some of
their applications.

A simple model for [[Machine Translation]] is introduced as a linear transformation between word embeddings.

[[Locality-sensitive Hashing]] is also introduced as a way to encode documents and enabling quick searches by looking up hash tables.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/1-classification-vector-spaces/week4-machine-translation-document-search.ipynb)

Explores the aforementioned topics.
