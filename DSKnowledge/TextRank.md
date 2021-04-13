Given a document, __TextRank__ is a [[NLP|NLP]] algorithm that returns a list of keywords that summarize the contents of the document, together with scores.

### Implementations

[[Gensim]] has an implementation:

```python
from gensim.summarization import keywords

text = "My document"

kws = keywords(text, scores=False, deacc=True)
```