This course is based on the study of probability models for NLP tasks such as [[autocorrection]] or
[[part-of-speech tagging]]. Its highlight is the introduction of [[Hidden Markov Models]] and the discussion
of neural word embeddings.

## Week 1. Autocorrect

The goal of this week is to explain how to design an autocorrect system, based on matching mispelled
words against a vocabulary corpus and studying the [[minimum edit distance]].

Two auxiliary notebooks are used as a helper, although the programming assignment already contains a
full example:

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week1-vocab-build-demo.ipynb): how to build a vocabulary.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week1-candidates-from-edits-demo.ipynb): how to choose candidates for correction.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week1-autocorrect.ipynb)

The problem is to code an autocorrect system, based on probabilities around the edit distance.

## Week 2. Part-of-speech tagging and Hidden Markov Models

The main subject of the week is [[Hidden Markov Models]]. They are introduced in relation to the problem
of [[part-of-speech tagging]].

In order to find the sequence of hidden states with maximum probability, the [[Viterbi Algorithm]] is discussed.

Several helpers are introduced in order to be able to manipulate a vocabulary with PoS tags:

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week2-tagging-vocabs.ipynb): creating a vocabulary out of a corpus.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week2-numpy-tags-demo.ipynb): creation and manipulation of matrices using numpy.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week2-pos-tagging.ipynb)

The problem is to code a part-of-speech tagger using a hidden Markov model and the Viterbi
algorithm.

## Week 3. Autocomplete and language models

The [[N-grams]] model is introduced and used in order to code an [[autocompletion]] system, based on count
probabilities.

There are three auxiliary notebooks for parts of the matter:

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week3-ngrams-demo.ipynb): on preprocessing related to n-grams.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week3-language-model-demo.ipynb): on building the count matrix, probability matrix
  and evaluating the language model.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week3-out-of-vocab-demo.ipynb): on how to deal with tokens which are not in the
  vocabulary.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week3-autocomplete.ipynb)

Build an autocomplete system based on n-grams and evaluate it using the perplexity metric.

## Week 4. Word embeddings with neural networks

The [[Word2Vec]] model with [[continuous bag-of-words]] is introduced and discussed.

Several notebooks are used as helpers. Remarkably, no fancy libraries ([[TensorFlow]], [[Keras]]) are used;
the algorithm (and the [[gradient descent]]) are coded straightaway in [[Numpy]].

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-data-prep-demo.ipynb): on data preparation.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-model-architecture-demo.ipynb): on the architecture of the CBOW model.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-cbow-training-demo.ipynb): on training the CBOW model.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-word-embedding-extraction-demo.ipynb): on extracting word vectors from a
  trained model.

- [Ungraded practice notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-practice-notebook-demo.ipynb).


### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/2-probabilistic-models/week4-word-embeddings.ipynb)

Using only [[Numpy]], code a [[continuous bag-of-words|CBOW]] model, train it and extract [[word embeddings]] out of it.

