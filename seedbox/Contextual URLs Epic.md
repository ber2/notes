# Summary

This is a contextual pipeline whose task is to:

1. Read URLs from a given source
2. Filter them and keep those which match a given set of keywords
3. Push the list of URLs away to a partner

__Reading URLs__: these need to be URLs where we can show an impression. Initially, we are working with our own first party impression data to extract URLs. We have reached an agreement with a new partner, [[Mediagrid]], by which they will send us _enriched [[BidStream]] data_, which will provide a much richer stream of URLs.

__Filtering URLs__: the rules for matching are precisely the same that we use for the [[URL Audiences]] job: given a list of keywords, a URL is a match provided it contains the keyword. If the keyword consists of two or more words, then both keywords must appear consecutively in the URL.

__Pushing URLs__: the list of URLs that have matched our keywords is sent to a partner for targeting. At the moment, we push URLs to the [[Xandr RTSS]] API service. It works in a very similar fashion as the API where we push segments. For this reason, we expect to be able to re-use a lot of the existing code in the [[dsp-sync]] service, or even to clone the service with minor changes.

# POC
Marçal implemented an initial POC in [RED-191](https://hybridtheory.atlassian.net/browse/RED-191) by importing code from the urlAudiences JAR, and reusing the [[dsy-sync]] python code.

# Epic planning

Tasks that need to be implemented (and data passed on):

- Query platform API, get list of datapoints and post it to S3. [SAS-11508](https://hybridtheory.atlassian.net/browse/SAS-11508)
- Spark job that loads the datapoints, loads impression URLs, matches URLs to keywords, and saves positive URLs away to S3.
- Job/Service that watches the S3 endpoint where we save results, fetches new data and pushes it to the [[Xandr RTSS]] API.

## What we have so far

- A Scala/Spark job that given a single keyword as an Airflow variable, stores a CSV with the list of impression URLs that have matched that keyword.

## Incremental steps to add value

- Single audience, multiple keywords.
- Read platform metadata directly.
- Multiple audiences, multiple keywords.