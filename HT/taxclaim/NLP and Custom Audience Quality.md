# NLP and Custom Audience Quality
In this strand of work we describe how we have used Natural Language Processing together with web scraping at scale in order to improve the scale and quality of our campaign audiences.

## State of affairs

### Custom audiences

Keyword-based audiences are built by specifying a list of keywords and identifying users who have browsed on websites related to those keywords. They are a common method to deliver campaigns at scale in AdTech, since they allow the identification of segments of users having a behaviour that expresses a common interest.

At [[Hybrid Theory]], we acquire and process large volumes of data through our partners. We build keyword-based audiences using an in-house developed product, [[Custom Audiences]], which is available in our Ad trading [[Platform]].

In this way, traders can define an audience in the Platform by describing a list of keywords. The URLs of recent events are then processed in order to verify whether they contain any of the given keywords. Users having visited any of these URLs are then put in the segment, so that they may be targeted in a campaign.

### Problems

URLs most often do not contain enough information about the website contents. To give an example, consider the following URL:

```
https://www.bbc.com/news/world-europe-56675290
```

It is not possible to deduce that this website contains a piece of news about the auction of a Caravaggio painting solely by looking at the URL. Thus, we would miss the chance to add users visiting this website to an audience composed of art lovers.

The procedure we follow in order to relate URLs to keywords is a _tokenization_, which consists of breaking up the URL into individual words (known as _tokens_), and then discarding some which are obviously not useful. In the case of the URL above, we would end up with a list such as:

```
bbc, news, world, europe
```

If we were to manually decide whether the given URL is relevant, we would access it, look at its contents and then decide to include it in our list of valid URLs because of the topics discussed.

We set out to automatize this procedure by using [[Natural Language Processing]] (NLP) techniques. The steps to replicate the above procedure would be:

- Send a request to the URL.
- Extract the contents from the request response.
- Clean up the HTML code into a document.
- Process the document using an NLP algorithm.

For the latter point, we will focus on algorithms that, given a document, 

The easiest way to obtain keywords is to tokenize the URL. This is what we do currently. Many URLs have tokens which are not informative enough; we trade off, however, on the  small extracting cost  and this approach trades off on the fact that this information is available in the events themselves.

If we were given the contents of the URL as a text document, then we could apply Natural Language Processing techniques. This would allow us to establish a relationship between keywords and URLs of a finer quality. For example:

-   Sentiment analysis methods could be used to detect whether a keyword features in a URL positively or negatively.
    
-   Named Entity Recognition methods could be applied to detect URLs related to specific people, places, etc.

Obtaining the contents of a URL involves sending an HTTP request and then processing the response. After the response is obtained, the HTML code needs to be cleaned into readable text, which can be then used for NLP purposes. And the challenge is doing this at scale, for several million URLs at once.

## Advances

- Data Quality Analysis
- Keyword Extraction
- Scraping
- NLP

## Encountered uncertainties

## Proposed solutions

