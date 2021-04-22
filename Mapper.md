The __Mapper__ is a [[HT]] product that is at the core of Engineering. It has a real-time component, which tracks cookie passbacks with timestamps, and a batch scheduled component.

## Real-time component

## Batch scheduled component

This, as the user-ids-mapper job, looks at what first-party and third-party ids have been seen together in browsing activity.

Each id is treated as a node, and each call as an edge. The connected components of the resulting [[Identity Graph]] are computed and stored.

## Universal ID

The connected components of the graph are labelled and used as a universal ID.