This is a project page regarding the creation of a data pipeline designed to be purely contextual.

Such a pipeline would consist of the following steps:

1. Receiving and processing URLs from bidding data from an external partner.
2. Using models to determine URL _segments_, that is: which set of URLs may be targeted for each campaign. This may include several ways of fabricating these sets, just like in the case of user segments:
    - __Retargeting__. URLs that have already driven conversion
    - __Lookalikes__. URLs that score very well in a conversion predictive model
    - __Keyword matches__. URLs matching a given keyword, regardless of whether it is present in the URL or extracted as an NLP entity from the URL contents
 3. Sending these sets of URLs to an external partner (an SSP) that can orchestrate campaigns based on this information.
 
 We treat each issue separately, bearing in mind what the obstacles are.

## 1. Enriching Bid Data

At this stage, we need to have access to URLs 

At this stage, it is important to be able to access a data provider that can give us access to the URLs available in the SSP inventory for targeting purposes.

## 2. ML Models based on URLs

A contextual signal can only be expected to have the following features:
- __Timestamp__. Should probably be employed more as a campaign setting than a learning feature, at least initially.
- __User agent__. Should be enriched to the point of extracting the device and/or operating system.
- __IP__. Should be enriched to allow for a broad sense of location, namely city and region/city. Narrow IP based geolocation beyond this is infamously faulty.
- __URL__.

The IP should be enriched enough to allow for a broad sense of location, namely country and region/city. Similarly, the User Agent should be enriched to the point of extracting the device and/or operating system information.

Similarly, timestamp considerations should be employed

## 3. URL based campaigns