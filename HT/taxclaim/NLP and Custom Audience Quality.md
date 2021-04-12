In this strand of work we describe how we have used Natural Language Processing together with web scraping at scale in order to improve the scale and quality of our campaign audiences.

## State of affairs

### Custom audiences

Keyword-based audiences are built by specifying a list of keywords and identifying users who have browsed on websites related to those keywords. They are a common method to deliver campaigns at scale in AdTech, since they allow the identification of segments of users having a behaviour that expresses a common interest.

At [[Hybrid Theory]], we acquire and process large volumes of data through our partners. We build keyword-based audiences using an in-house developed product, [[Custom Audiences]], which is available in our Ad trading [[Platform]].

In this way, traders can define an audience in the Platform by describing a list of keywords. The URLs of recent events are then processed in order to verify whether they contain any of the given keywords. Users having visited any of these URLs are then put in the segment, so that they may be targeted in a campaign.

We very often encounter URLs which do not contain enough information about the website contents. To give an example, consider the following URL:

```
https://www.bbc.com/news/world-europe-56675290
```

It is not possible to deduce that this website contains a piece of news about the auction of a Caravaggio painting solely by looking at the URL. Thus, we would miss the chance to add users visiting this website to an audience composed of art lovers.

The procedure we follow in order to relate URLs to keywords is a [[Tokenization]], which consists of breaking up the URL into individual words, and then discarding those which are obviously not useful because of their being extremely common (these words are referred to as [[Stopwords]]). In the case of the URL above, we would end up with a list such as:

```
bbc, news, world, europe
```

If we were to manually decide whether the given URL is relevant, we would access it, look at its contents and then make a decision about its inclusion in our list of valid URLs based on the topics appearing in the text.

## Problems and uncertainties

We want to enhance keyword extraction methods so that we can activate more URLs when constructing our audiences.

The approach outlined above, consisting in checking the contents of websites associated to URLs, may be automatized by using [[Natural Language Processing]] (NLP) techniques. The steps to replicate the above procedure in an automated fashion would involve a script that:

1. Sends a request to the URL.
2. Extracts HTML code from the response body.
3. Cleans up the HTML code into a plain text document.
4. Processes the document using an NLP algorithm.

More generally, we set out to study further methods to associate a list of keywords to a given URL.

### Network transfer

The range of distinct URLs that we observe in a given day oscillates in the 70 million range. The main engineering problem to solve when working towards an automatized NLP solution is how to perform the URL lookups at scale for this volume.

Each URL lookup involves establishing an HTTP connection and, when performing lookups sequentially, the vast majority of CPU time is spent waiting for responses. As a matter of fact, this is a classic example of a problem which benefits enormously from [[parallelization]] (more precisely, [[concurrent]], [[asynchronous]] parallelization).

If we parallelize such a computation on a single machine, however, we will soon encounter another hurdle: the limit of outgoing connections is bounded. 

Therefore, it is necessary to find a clean approach to parallelizing the access to website contents.

### Header vs whole document

The website head matter contains, under the `<title>` header, a short title sentence which often can serve as a documentr summary.

Extracting that sentence prior to parsing the rest of the document could provide enough information to allow us to stop looking at the rest of the document.

Hence, this is a possibility to explore.

### Document extraction

In order to obtain a plain text document from the HTML content coming from a website, we need to parse the HTML code, clear away any HTML tags, decide which parts of the text would correspond to the visible text, and keep them.

Luckily, this is a very common problem. In [[Python]], the most popular tool for HTML clean-up is the [[Beautiful Soup]] library, which we intend to use.

In this case, we will only need to validate whether this library performs well under our constraints.

### Keyword extraction

The procedure of extracting keywords from a given text document is a well-known [[Natural Language Processing|NLP]] text summarization task. There are many open source implementations available in well-known, easily accessible libraries such as [[NLTK]], [[SpaCy]], [[Gensim]] or [[Stanza]].

To name but a couple options, [[TextRank]] and [[RAKE]] are two well-known keyword extraction algorithms.

In this case, our problem is to validate the performance of these algorithms in regards to our particular problem.

### Other NLP techniques 

There are further [[Natural Language Processing|NLP]] techniques that could be applied in order to extract information of a better quality out of website contents. In particular:

- [[Sentiment Analysis]] methods could be used to detect whether a keyword features in a URL positively or negatively. This would allow us to discern between websites expressing favourable or contrary opinions about a given subject.

- [[Named Entity Recognition]] methods could be applied to detect URLs related to specific people, places, etc.

Obtaining the contents of a URL involves sending an HTTP request and then processing the response. After the response is obtained, the HTML code needs to be cleaned into readable text, which can be then used for NLP purposes. And the challenge is doing this at scale, for several million URLs at once.

## Advances and Progress Made

- Keyword Extraction
- Scraping
- NLP

- Data Quality Analysis


## Future work

