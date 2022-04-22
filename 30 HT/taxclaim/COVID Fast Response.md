In this strand of work we describe how we reacted in order to provide a fast response to the [[COVID-19]] crisis, enabling our teams across the globe to keep their customers up to date.

## State of affairs

The COVID-19 pandemic broke out during late 2019 in China. Our customers in Asia and the Pacific were the first ones to be affected by lockdowns, with the rest of our business regions (America, Europe, Middle East, Africa) following shortly after.

In this setting, the Engineering team was very quickly requested to leverage the volumes of data that we process on a daily basis in order to provide updated information to our customer-facing teams, with an overall purpose of leveraging the impact of the COVID pandemic in our campaigns.

Our existing reporting infrastructure is centered around the behaviour of very specific audiences, that is: users that have visited an advertiser's website. Moreover, we are constrained to a very specific context which is both geographical and temporal. It was clear to us that we needed to go beyond the capabilities available at the time.

## Problems and uncertainties

In the case at hand, we needed to expand our capabilities in several directions.

### Trend discovery beyond audience building

As discussed, when moving away from the comparatively small set of users having visited an advertiser's website to just any user seen in our [[third-party browsing data]] in general, the sheer volume of data to be parsed poses a challenge.

### Multi-region trends and cross-comparisons

We established the need to cross compare data from different countries and regions at different times, in order to examine what analogies could be established.

In particular, we wanted to know what lessons could be extracted from the early experience of Asian countries with lockdowns: what user behaviours were modified as a result of staying indoors and working from home first, and what changes could be observed later as lockdown restrictions eased.

### Noise treatment

The volume of data we process oscillates very much along time, for reasons that are alien to us, and beyond our control. Therefore, extracting a reliable signal from these events is a challenging task that cannot be limited to performing counts of numbers of users having exhibited a particular website.

### Existing available alternatives

We discarded looking at alternatives because of the need to provide a fast response was urgent.

## Advances and Progress Made

### Data democratization effort

We adopted [[Databricks]] as a reporting platform for the purposes of our response to COVID-19. There are several aspects of the functioning of Databricks as a platform that are very particularly suited to our goals:

- [[Data democratization]]. By this, we understand the capability of empowering any of the members of our organization to access the platform and interact with our data directly.
- Unlocking the power of [[Spark]] for large volume computations. In particular, at a [[Data Engineering]] level, being able to scale our clusters easily was crucial towards delivering our fast response.
- The ability to run code in [[Notebook]]s in a combination of different programming languages. During the COVID project we used [[Python]], [[Scala]] and [[SQL]], sometimes combined within the same notebook.
- The capability to add [[Markdown]] code and interactive [[Data Visualization]] of queries within a [[Notebook]]. This is most probably the reason why notebooks quickly became the preferred way in which reports were shared amongst different company teams.

### Time Series analysis

The noise problem in our input data called for a rigorous approach. Since we focused in the reporting of the evolution of the number of occurrences of certain terms along time, we borrowed from the existing corpus of knowledge in the area of [[Time Series analysis]].

Our research exposed that daily aggregations of our data form time series which are well behaved when decomposed using a [[Seasonal Decomposition]] approach. More precisely, using an [[Seasonal Decomposition]], we very quickly established that [[period]] of 7 days, following the obvious weekly pattern, allowed to extract a robust [[trend]].

These trends, which may be understood as a tidy representation of the fluctuation of the time series being studied, are still noisy and difficult to compare to each other. We found out that [[Indexing]] by centering data around a given date, giving that date the value 100 and scaling the rest of the time series in proportion was stable enough for periods of time between 30 and 45 days; this was sufficient for our purposes.

We used the [[Time Series analysis]] tools available in the [[Python]] [[statsmodels]] package.

### Adaptation of reports to custom requests

The COVID reports were very well received and put to good use as a showcase of our data processing capabilities, which set us apart from other competing small online advertising agencies.

Soon afterwards we started collaboration with our Sales team, as they saw an opportunity of using similar reports when pitching prospective clients, for which we did not have first-hand data available.

In this case, the problem was most often related to the difficulties of finding data related to certain advertisers given its scarcity and small online presence.

At this point, we used our [[Scraper]] to produce a corpus of keywords available to us from our [[third-party browsing data]] to be studied as an [[NLP]] corpus.

We were able to train [[Word2Vec]] models on this corpus. Such embeddings map words to vectors in a very high-dimensional [[Euclidean space]], and they are known for their capability of encoding deep semantic relationships in the geometry of the vector space.

By reducing dimensions using the [[UMAP]] technique, we were able to produce reports based on the visualizations of given concepts and their proximity to our advertisers.

### The Keyword-trends product

Main page: [[Keyword Trends]]

## Future work

After delivery of the [[Keyword Trends]] product, we are working on enhancing it and automatizing the display of trends in our own [[Platform]].

We are also currently working in the direction of producing a [[Dashboard]] showing keyword-based reports. In this way, our Sales Teams will unlock our excellent reporting capabilities and exhibit them to prospective customers, so that we can engage in a data-driven narrative prior to closing a deal and acquiring [[first-party browsing data]].