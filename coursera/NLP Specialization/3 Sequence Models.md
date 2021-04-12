## Week 1. Neural Networks for Sentiment Analysis

[[Neural Network]]s and [[Deep Learning]] are introduced, and then used to build a [[Sentiment Analysis]] [[binary classifier]] for tweets.

The libraries [[Trax]] and [[JAX]] are introduced.


There are three auxiliary notebooks available:

- [Notebook](./week1-trax-intro-demo.ipynb): introduction to `trax`.

- [Notebook](./week1-class-inheritance-demo.ipynb): introduction to python OOP (class inheritance, extending classes, etc).

- [Notebook](./week1-data-generators-demo.ipynb): how to build generators using `yield`.

### Programming assignment

- [Notebook](./week1-sentiment-with-NN.ipynb)

Serves as a gentle introduction to Trax by training a small neural network for sentiment analysis: it builds a binary classifier for analyzing tweets as either positive or negative.

## Week 2. Natural Language Processing with Sequence Models

RNNs are discussed with an emphasis on GRUs in order to build sequential models.
The assignment consists of using such a model in order to create a next-word generator trained on Shakespeare data.

There are a number of auxiliary notebooks demonstrating basic aspects:

- [Notebook](./week2-hidden-state-activation-demo.ipynb): hidden state activation.

- [Notebook](./week2-perplexity-with-jax-demo.ipynb): JAX as a fast numpy replacement and perplexity computations.

- [Notebook](./week2-rnn-gru-demo.ipynb): forward propagation on RNNs.

- [Notebook](./week2-gru-in-trax-demo.ipynb): a GRU model on Trax.

### Programming assignment

- [Notebook](./week2-deep-ngrams.ipynb)

Implementation of an end-to-end RNN model using Trax and a GRU.
The model is used to build a next-word generator trained on a Shakespeare corpus.

## Week 3. LSTMs and Named Entity Recognition

Long Short-Term Memory (LSTM) models are introduced and discussed.
They are applied to the practical problem of Named Entity Recognition (NER).

There are a few supporting materials.

- [Notebook](./week3-vanishing-gradients-demo.ipynb): on vanishing gradients.

- [Blog post](https://blog.paperspace.com/intro-to-optimization-in-deep-learning-gradient-descent/): Introduction to gradient descent.

- [Blog post](https://colah.github.io/posts/2015-08-Understanding-LSTMs/): Understanding LSTMs.

### Programming assignment

- [Notebook](./week3-lstm-ner.ipynb)

The architecture of a LSTM neural network is designed and the model is trained and tested on a NER task.

## Week 4. Siamese networks

Siamese networks are introduced.
These are models built using two identical networks that are eventually merged together.
Then, they are put to use for a task of detecting duplicate questions.

There are a couple of supporting notebooks.

- [Notebook](./week4-siamese-trax-demo.ipynb): Creating a Siamese model using Trax.

- [Notebook](./week4-triplet-loss-demo.ipynb): About the triplet loss metric.

- [Notebook](./week4-siamese-evaluation-demo.ipynb): Evaluation of Siamese models. 

### Programming assignment

- [Notebook](./week4-question-duplicates.ipynb)

On a the detection of question duplicates using a Siamese network on Trax.
