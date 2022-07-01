## Goal
As requested by [[Marçal Serrate]], work out a data product proposal involving Facebook’s Robyn package for Mixed Marketing Modelling and how to integrate it with our clients. In particular:

-   Propose integration steps and timing
-   Work out costs and a licensing pricing model

### Context framing
Hybrid Connect: connect all your marketing tools together, and combine them into a single data warehouse. Usaríem una eina preconfigurada: Fivetran. Hi ha eines ja preconfigurades i que donen molta 

Mix: Robyn

Lift: Experimentació de conversion lift i geo lift. [[GeoLift]]

Pricing: en funció del nombre d'origens a incloure (tracks).

Sortida: powerBI? (Marçal ja va fer alguns reports)
Mock up amb data app?
Storytelling


### References
The project itself: [[Robyn]].

[[Miriam Aumatell]] did an R&D story on the subject: [RED-186](https://hybridtheory.atlassian.net/browse/RED-186). Several results spawn out of it:
- [[Automated MMM with Robyn]]
- [[Robyn]]


## NOTES/TODO
- Hi ha a la documentació una manera d'executar el model des de docker.
Usar la configuració en el fitxer zip compartit al final d'aquesta issue: https://github.com/facebookexperimental/Robyn/issues/173

# Product proposal
We discuss a new hybrid consulting/automated product, Hybrid Connect, enabling Marketing Mixed Modelling for our customers.

Traditionally, MMM models have been costly endeavours, involving either a high amount of in-house development or the expertise of a consulting agent. The recent appearance of tools that significantly automatize key parts of the process provide us with an opportunity to offer MMM at fraction of the former cost and at a much lower time-to-market. 

## Measuring Campaign Effectiveness
### What is MMM?
Marketing Mix Modelling, [[MMM]], quantifies the incremental sales impact and ROI of all online & offline marketing strategies featuring:

- Privacy-friendly input data
- High degree of customization
- Enabling data-driven decision making

At a high level, an MMM analyzes the statistical evidence for strong relationships between the spend on different media channels and sales revenue. Traditionally, this has been a product either built in-house, or by a consulting agency.

We have identified an opportunity to help our customers perform MMM due to the appearance of tools that can significantly reduce the time-to-market especially in the areas of Data Governance and Model Generation.

These enhancements, discussed below, do not fully remove the need for the intervention of a Data Scientist or Analyst. However, they promise a very significant reduction in the amount of work involved in outputting an MMM. So, our gap of opportunity in here consists of a drastic reduction in development cost and delivery time of the MMM, instead of developing a fully automated product.

There are several technical challenges to solve when building an MMM:

1. Data Governance. To coalesce and validate data regarding independent media channels into a single source of truth. In many companies, this is already a titanic task, with marketing data spread across different teams and silos.

2. Data Availability. To ensure that sufficient data is available in order to be able to extract statistically relevant insights. On top of the first point, this often involves having a good data culture.

3. Model generation. Fitting an MMM model and extracting conclusions from it is a hard task.

On one hand, a range of marketing-specific Extract-Load-Transform (ELT) tools have appeared, allowing unified and easy access to data in different media channels. Thanks to them, the issue of Data Governance has largely evolved from requiring an expensive in-house solution to being a commodity. We propose to use tools such as [FiveTran](https://www.fivetran.com/) or [AdVerity] (https://www.adverity.com/) to this end. These tools also have the advantage of providing a transparent integration between our clients, third-party media platforms and ourselves.

On the other hand, exciting new AI models have been open-sourced recently, allowing a new approach to MMM. In particular, the Facebook AI team have released [Robyn](https://facebookexperimental.github.io/Robyn/).

## An overview of Robyn
The Robyn package has been developed by Facebook AI as a way to automatize much of the statistical analysis involved in fitting an MMM.

At its core, an MMM revolves around the explainability of a linear regression model relating independent variables (the reach and spend in each media channel) to a dependent variable (most often revenue). A posteriori, when doing budget allocation, the need to understand the causal relationships amongst different channels arises.

Although this kind of model is simple, there is a considerable amount of feature engineering in order to have better explainability:

- Usage of the Facebook Prophet library, which automatically decomposes time series to discover trends, seasonalities and holiday patterns.
- Usage of the Facebook Nevergrad package to optimize and fine-tune hyperparameter choices. This results not in the presentation of a single optimal model, but a Pareto front of optimal models, for an analyst to work with.
- The detailed implementation of two Marketing-specific features to measure the effect of actions on a given media channel:
    - AdStock, the idea that the impact of impressions is not immediate, but has a decaying but lasting effect in time. In other words, consumers have some memory and the effect of seeing an impression does not wear off immediately. The model is capable of expressing this feature in a very configurable way by using two different families of decaying functions that may be adjusted via hyperparameters (geometric and Weibull).
    - Saturation, the idea that excessive amounts of impressions on a media channel have large costs with diminishing returns. Hill functions are used to this end.

Furthermore, as mentioned above, there is a separate tool that estimates the effects of budget reallocation across different channels.

Although all of these features are very promising, the Robyn package itself has not reached a point of maturity where an automatized pipeline could be built. The library itself is liable to changing and a significant amount of interaction and fine-tuning is necessary.

For this reason, we propose to have a Data Scientist involved in the process of fitting the model, selecting models in the Pareto front with good adherence to expectations, and choosing which budget reallocation actions could be prescribed.


## Data inputs
In order to be able to train an MMM using Robyn, it is advised to supply at least two years worth of impressions/reach and spend for each media channel under consideration, aggregated at a weekly level (104 data points).

By reducing the number of channels used as model inputs, one should be able to reduce this amount down to a year (52 data points).

Alternatively, for campaigns with very short time spans, daily aggregated events could be considered, for data comprising a span of up to four months. However, our initial experiments have shown that the model becomes noisy in such cases.

The general rule of thumb here, as discussed by the Robyn package authors themselves, is that one should have about 7 times as many data points than media channels.

Most customers will have a very hard time putting together a dataset with the aforementioned features, due to poor Data Governance within their own organisations and the fact that this data is typically split in different silos.

In order to reduce the friction when collecting this data, we propose using a third-party marketing-oriented ELT platform. Tools such as FiveTran or AdVerity provide several advantages:
- A wide range of plugins to connect to most media channels (particularly online ones: DSPs, social networks, etc).
- Shared, transparent access for our customers
- Separation of responsibilities: customers can configure plugins for individual channels using their own credentials, while we could arrange for coalesced data to be dumped into our own cloud storage following a consistent schema.

## Outputs
The main output of an MMM is the storytelling around how well spent marketing money is across different channels.

There are two main components to report about:
1. Channel-specific ROI. The explainability of the MMM itself provides an interpretation of the performance of marketing campaigns across different channels.
2. Prescriptions. The budget allocator tool allows to prescribe actions with a forecasted positive effect.

The first output will very likeyl be automatized into a report including data visualizations (a good array of visualizations is provided by the Robyn package itself. The second output is much more delicate, as consumers are generally very reticent to implement actions prescribed by an AI; one needs to be very subtle when introducing this kind of recommendations, and supply a large amount of evidence.

As far as results delivery is concerned, we have several tools at our disposal:
- PowerBI
- Data apps
- Enhanced reporting platform (a new product proposal in itself)
- Storytelling emails

## Timings
It is very hard to make an accurate proposition of how long it should take us to deliver a report on a multichannel campaign using Robyn without conducting an experiment. Initially, 30-45 days seems like a reasonable approach

## Pricing
--

## Next steps
- Test AdVerity (Rhys Williams is already using the platform internally for the new runrates)
- Test FiveTran
- Start conversations to engage one or two of our customers in an end-to-end experiment 
- Supply code that is capable to perform as much of an end-to-end run as possible within a docker container (using Rscript / RStudio)

## Competitors
- Analytic Edge. [web](https://www.analytic-edge.com/#)
- Facebook partner directory: [web](https://www.facebook.com/business/partner-directory/search?solution_type=measurement&ref=pd_home_hero_cta&capabilities=Marketing%20Mix%20Modeling)

## References
- [Robyn](https://facebookexperimental.github.io/Robyn/) homepage
- 
- A [confluence page](https://hybridtheory.atlassian.net/wiki/spaces/EN/pages/3350430127/2022-03+Robyn), containing the output of our own original experiments with the Robyn package ([RED-186](https://hybridtheory.atlassian.net/browse/RED-186))
- [Analysts guide to MMM](https://facebookexperimental.github.io/Robyn/docs/analysts-guide-to-MMM), a comprehensive explanation of the technical details behind Robyn's approach to MMM.