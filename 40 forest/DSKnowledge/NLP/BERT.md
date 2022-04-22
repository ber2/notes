---
aliases: [Bidirectional Encoder Representations from Transformers]
---

__Bidirectional Encoder Representations from Transformers (BERT)__ is a [[Deep Learning]] [[NLP]] model of the [[Transformer]] family.

It makes use of [[Transfer Learning#Fine-tuning]].


The base model, `BERT_base`, has an architecture composed of:
- 12 layers (transformer blocks)
- 12 attention heads
which adds up to
- 110 million parameters

The newer models, such as [[GPT3]], use orders of magnitude more parameters.

## Pre-training phase

Trains on multiple tasks with unlabelled data. We use the so-called [[Mask Language Modelling]]:
- Choose 15% of the tokens at random
- Mask them 80% of the time
- Replace them with a random token 10% of the time
- Keep as is 10% of the time

Multiple masked spans are allowed in the sentence.
Next sentence prediction is also used.

## Objective

As an input, BERT takes three embeddings of the tokens:

- Input embeddings. They have a \[CLS\] token to indicate the beginning of a sentence and a \[SEP\] token to indicate the end of a sentence
- Segment embeddings. They allow to indicate the separation between two sentences.
- Positional embeddings. They allow to indicate a word's position in the sentence.

Each of these embeddings allows for a different task to be trained (next word prediction, next sentence prediction).

Each of the tasks trains on a different loss function.

- Multi-Mask LM is trained on a [[Cross Entropy Loss]].
- Next sentence prediciton is trained on a [[Binary Loss]].

Both are combined by adding them up.


## Fine-tuning

Once trained enough, we transfer learn to the task at hand. The results at hand are often state of the art.

- Hypothesis / Premise: MNLI. We pass hypotheses as the first sentence, premises as the second one.
- NER. We pass text as the first sentence, tags as the second one.
- SQuAD (Question answering). We pass questions as first sentence, answers as second sentence. 