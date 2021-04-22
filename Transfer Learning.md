__Transfer Learning__ is the art of recycling a model that has been trained for a task by using what it has learned in order to tackle a different task.

Typically used in [[NLP]], the usage of transfer learning greatly reduces training time.

There are two approaches to transfer learning:

## Feature-based

Pre-train a model and use the learned weights of that model as features to train a new model.

[[Word embeddings]] are a very good example of this, as word vectors are the learned weights from another task such as the [[CBOW]]; predicting the missing word in a sentence.

Many [[Sequence models]] in [[NLP]] use an embedding layer as a way to encode text into features.

## Fine-tuning

In this approach, first you pre-train a model on a given task and then you re-use that model for a different task, directly.

Re-using the first model's weights as an initialization for the second one can save huge amounts of training time. One is then allowed to add extra layers to the model and fine-tune for the task at hand.
