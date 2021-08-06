__Performant Domains__ is a [[HT|Hybrid Theory]] product that aims to offer a simple targeting solution based on the domains where impressions are shown.

The main idea is to automatically supply a list of domains to Xandr to be targeted in relationship to their performance for a given advertiser.

Given an advertiser id with a running campaign:
1. Look up the domains where impressions have been served in the last 7 days for the advertiser.
2. Generate a list of domains.
3. Push the list via the Xandr's [Domain List Service](https://wiki.xandr.com/pages/viewpage.action?spaceKey=adnexusdocumentation&title=Domain+List+Service).

The only step which is not straightforward engineering and really deserves some thought is the second one, about how to generate the list of domains. This was originally explored in [RED-147](https://hybridtheory.atlassian.net/browse/RED-147).

## Domain selection

A conversion metric, such as CTR, should be selected. Then, 

### References 

- Product team's confluence pages about the subject [here](https://hybridtheory.atlassian.net/wiki/spaces/PROD/pages/2975563777/2021.06+Contextual+Performant+Domains+Product) and [here](https://hybridtheory.atlassian.net/wiki/spaces/PROD/pages/2481225729/Done+2021.03+Performant+Domains+for+Contextual+Targeting).