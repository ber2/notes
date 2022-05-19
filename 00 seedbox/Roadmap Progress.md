---
tags: [roadmap, HT, evergreen]
---

This evergreen note will serve the purpose of tracking pending stuff in the roadmap. Particularly stuff that I should keep track of.


## Product
### Data retention
- [x] [SAS-11561](https://hybridtheory.atlassian.net/browse/SAS-11561) Load anonymised data in events partitioner
- [ ] [SAS-11562](https://hybridtheory.atlassian.net/browse/SAS-11562) Xandr loglevel loader anonymises data.
- [ ] [SAS-11567](https://hybridtheory.atlassian.net/browse/SAS-11567) Drop old data from af-reservoir
- [ ] [SAS-11567](https://hybridtheory.atlassian.net/browse/SAS-11567) Drop old data from af-xandr-raw
- [ ] [SAS-11569](https://hybridtheory.atlassian.net/browse/SAS-11569) af-events to Glacier
- [ ] [SAS-11570](https://hybridtheory.atlassian.net/browse/SAS-11570) af-xandr to Glacier

- [ ] [SAS-9376](https://hybridtheory.atlassian.net/browse/SAS-9376) Delete old metastore `test-databricks`

### Contextual URLs
- [x] [SAS-11834](https://hybridtheory.atlassian.net/browse/SAS-11834) Encoding of non-ascii characters before pushing to RTSS.

## Maintenance
### Lookalikes V2
We are currently implementing the final stages of the migration, only the _segment sync_ task needs to be done. There is a main path aimed at producing segments, and a side path aimed at reporting by-products.

#### Main path
- [x] [RED-228](https://hybridtheory.atlassian.net/browse/RED-228) Compare performance of old and new models
- [ ] [SAS-11815](https://hybridtheory.atlassian.net/browse/SAS-11815) Placeholder for replacing one pipeline by the other
- [x] [SAS-11838](https://hybridtheory.atlassian.net/browse/SAS-11838) Select top-scoring users as described.
- [ ] [SAS-12023](https://hybridtheory.atlassian.net/browse/SAS-12023) LAL-V2 scoring task parallelization

#### Side path
- [x] [SAS-11506](https://hybridtheory.atlassian.net/browse/SAS-11506) Save token weights table (aka supamodels)
- [ ] [SAS-11032](https://hybridtheory.atlassian.net/browse/SAS-11032) Spike about new reporting by-products

### Shrink Redshift Computing
- [x] [SAS-11368](https://hybridtheory.atlassian.net/browse/SAS-11368) Retain only 3-4 months of first party data in Redshift Computing
#### Category segments in Databricks
- [x] [SAS-11788](https://hybridtheory.atlassian.net/browse/SAS-11788) Spike to plan it out
- [x] [RED-225](https://hybridtheory.atlassian.net/browse/RED-225) Overlap reports via minhash

## Tech debt
### Airflow 2.0 Migration
- [ ] [SAS-11789](https://hybridtheory.atlassian.net/browse/SAS-11789) Remove custom metaclasses from operators.
- [ ] [SAS-11795](https://hybridtheory.atlassian.net/browse/SAS-11795) Move logging config.
- [ ] [SAS-11794](https://hybridtheory.atlassian.net/browse/SAS-11794) Changes in import paths
- [ ] [SAS-11796](https://hybridtheory.atlassian.net/browse/SAS-11796) No additional arguments for `BaseOperator`.
- [ ] [SAS-11812](https://hybridtheory.atlassian.net/browse/SAS-11812) Placeholder for actual version migration.

### Databricks management
- [x] [SAS-11687](https://hybridtheory.atlassian.net/browse/SAS-11687) Spike about user groups
- [x] [SAS-11688](https://hybridtheory.atlassian.net/browse/SAS-11688) Spike about cluster management
- [x] [SAS-11775](https://hybridtheory.atlassian.net/browse/SAS-11775) Create user groups

### Misc
- [x] [SAS-11817](https://hybridtheory.atlassian.net/browse/SAS-11817) POC to stop using PyPIcloud

### Alpha environment data sampling
- [ ] [SAS-11287](https://hybridtheory.atlassian.net/browse/SAS-11287) Sample enricher output

## R&D Stories
- [x] [RED-223](https://hybridtheory.atlassian.net/browse/RED-223) S3-logs into incubator
