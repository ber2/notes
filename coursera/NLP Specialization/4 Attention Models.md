# Week 1: Neural Machine Translation with Attention

The goal in this week is to implement a [[Machine Translation]] [[Neural Network]] using a [[seq2seq]] model.

[[Attention]] is used as a way to keep track of previous words in the input data, and [[teacher forcing]] is used as a means to speed-up training.

The [[BLEU score]] and [[ROUGE metric]] are discussed for evaluation.

- [Post](https://towardsdatascience.com/what-is-teacher-forcing-3da6217fed1c): What is teacher forcing?

- [Notebook](https://github.com/ber2/coursera/blob/feature/nlp-course-4/nlp-specialization/4-attention-models/week1-stack-syntax-in-trax-demo.ipynb): about stack syntax in the [[Trax]] library.

- [Notebook](https://github.com/ber2/coursera/blob/feature/nlp-course-4/nlp-specialization/4-attention-models/week1-bleu-demo.ipynb): about calculating [[BLEU score]]s.

### Programming assignment

- [Notebook](https://github.com/ber2/coursera/blob/feature/nlp-course-4/nlp-specialization/4-attention-models/week1-nmt-with-attention.ipynb)

It codes a Neural Machine Translator using a seq2seq model (made up of LSTMs with attention), and teacher forcing.

- [Post](https://medium.com/@rashmi.margani/how-to-speed-up-the-training-of-the-sequence-model-using-bucketing-techniques-9e302b0fd976) discussing **bucketing**: how to efficiently batch sequence data according to sentence length.


# Week 2: Attention and text summarization


### References

- Exploring the Limits of Transfer Learning with a Unified Text-to-Text Transformer. [arXiv](https://arxiv.org/abs/1910.10683)
- Reformer: The Efficient Transformer. [arXiv](https://arxiv.org/abs/2001.04451)
- Attention Is All You Need. [arXiv](https://arxiv.org/abs/1706.03762)
- Deep contextualized word representations. [arXiv](https://arxiv.org/abs/1802.05365)
- The Illustrated Transformer. [post](http://jalammar.github.io/illustrated-transformer/)
- The Illustrated GPT-2 (Visualizing Transformer Language Models). [post](http://jalammar.github.io/illustrated-gpt2/)
- BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding. [arXiv](https://arxiv.org/abs/1810.04805)
- How GPT3 Works - Visualizations and Animations. [post](http://jalammar.github.io/how-gpt3-works-visualizations-animations/)
