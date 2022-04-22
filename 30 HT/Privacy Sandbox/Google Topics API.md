---
aliases: [FLoC 2.0]
---

## TL;DR

## Intro

This is Google's latest proposal in the [[Privacy Sandbox]] product line, and a direct descendant of the now-defunct _[[FLoC]] proposal_. Hence, we may think of this __Topics API__ as a __FLoC 2.0__.

## High level description

When a pixel fires on [[Google Chrome]], we will be able to call this API. The API will respond with up to three categories that the user is interested in.

Although there is an initial proposal, as of 2022-01-26 much of the detail is still under discussion. However, two relevant facts seem to be fixed and are unlikely to change:

1. The computation of the user's categories of interest will be based on the user's previous browsing behaviour.

2. The categories will not be computed from the whole of the user's history, but rather from pages from which we have already called the API.

Point 1 implies that this proposal is a __cookieless interest-based advertising__. Point 2 hints at the fact that it may pay to have third-parties calling our pixel.

### Computation of user categories of interest

First, websites are categorized using a [[ML|Machine Learning]] [[multilabel classifier]]. Details of the algorithm have not been made public.

The number of labels is subject to variation: from several hundred (350 in the first description), up to a full fledged [[IAB classification]]. This list is expected to be human-curated and public. Furthermore, sensitive categories will be deliberately kept out of the list, to protect users against discrimination.

When users visit websites, their interest in the category of the website is registered. On each week, the top 5 topics for a given user are computed in-browser. An additional 6th topic, selected at random, is appended to the list.

### Calling the API on a browsing event

When we call the API on a browser event, we will receive an answer with up to three topics from the list computed above, one for each of the last three weeks.

The topic returned by the API is fixed for each pair (user, website).

Only callers that have observed the user visit a site about a given topic in the past three weeks can receive the topic as a response. If this is not the case, the topic will not be returned for 

When the user
3. A list of categories of interest for a given user is made available to advertisers.
4. Sensitive categories are left out to avoid user discrimination (gender, race, religion, etc).
5. Moreover, users can manually deactivate specific categories for which they do not want to see ads. Finally, they can opt-out completely.

## The Topics API




This API labels each website with recognizable high level topics. These labels are to be selected from a human-curated, publicly visible list of around 350 labels. The list of topics should not include sensitive categories.

## References

- Google Blog announcement [post](https://www.blog.google/products/chrome/get-know-new-topics-api-privacy-sandbox/)

- Proposal [homepage](https://privacysandbox.com/proposals/topics)

- Technical specs [README](https://github.com/jkarlin/topics)