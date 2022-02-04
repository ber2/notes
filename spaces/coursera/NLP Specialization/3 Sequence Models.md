## Week 1. Neural Networks for Sentiment Analysis

[[Neural Network]]s and [[Deep Learning]] are introduced, and then used to build a [[Sentiment Analysis]] [[binary classifier]] for tweets.

The libraries [[Trax]] and [[JAX]] are introduced.

There are three auxiliary notebooks available:

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week1-trax-intro-demo.ipynb): introduction to [[Trax]].

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week1-class-inheritance-demo.ipynb): introduction to [[OOP]] in [[Python]] (class inheritance, extending classes, etc).

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week1-data-generators-demo.ipynb): how to build generators using `yield`.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week1-sentiment-with-NN.ipynb)

Serves as a gentle introduction to Trax by training a small neural network for sentiment analysis: it builds a binary classifier for analyzing tweets as either positive or negative.

## Week 2. Natural Language Processing with Sequence Models

[[RNN]]s are discussed with an emphasis on [[GRU]]s in order to build sequential models.
The assignment consists of using such a model in order to create a [[next-word generator]] trained on Shakespeare data.

There are a number of auxiliary notebooks demonstrating basic aspects:

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week2-hidden-state-activation-demo.ipynb): hidden state activation.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week2-perplexity-with-jax-demo.ipynb): [[JAX]] as a fast [[Numpy]] replacement and [[Perplexity]] computations.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week2-rnn-gru-demo.ipynb): [[forward propagation]] on [[RNN]]s.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week2-gru-in-trax-demo.ipynb): a [[GRU]] model on [[Trax]].

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week2-deep-ngrams.ipynb)

Implementation of an end-to-end [[RNN]] model using [[Trax]] and a [[GRU]].
The model is used to build a [[next-word generator]] trained on a Shakespeare [[corpus]].

## Week 3. LSTMs and Named Entity Recognition

[[LSTM|Long-Term Short Memory]] (LSTM) models are introduced and discussed.
They are applied to the practical problem of [[NER|Named Entity Recognition]] (NER).

There are a few supporting materials.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week3-vanishing-gradients-demo.ipynb): on the [[vanishing gradient]] problem.

- [Blog post](https://blog.paperspace.com/intro-to-optimization-in-deep-learning-gradient-descent/): Introduction to [[gradient descent]].

- [Blog post](https://colah.github.io/posts/2015-08-Understanding-LSTMs/): Understanding [[LSTM]]s.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week3-lstm-ner.ipynb)

The architecture of a [[LSTM]] [[Neural Network]] is designed and the model is trained and tested on a [[NER]] task.

## Week 4. Siamese networks

[[Siamese network]]s are introduced. Then, they are put to use for a task of [[Duplicate detection]] in a set of questions.

There are a couple of supporting notebooks.

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week4-siamese-trax-demo.ipynb): Creating a [[Siamese network]] model using [[Trax]].

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week4-triplet-loss-demo.ipynb): About the [[triplet loss metric]].

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week4-siamese-evaluation-demo.ipynb): Evaluation of [[Siamese network]] models. 

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/master/nlp-specialization/3-sequence-models/week4-question-duplicates.ipynb)

On the [[Duplicate detection]] in a set of questions using a [[Siamese network]] on [[Trax]].
