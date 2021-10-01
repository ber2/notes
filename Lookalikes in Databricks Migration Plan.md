This document details a migration plan to upgrade the [[Lookalikes]] codebase from its current status to Databricks.

Particular changes that we are interested in:
- To move away from AWS [[EMR]] and into [[Databricks]].
- To move away code written in [[Apache Pig]] to [[Scala]] 2.X.
- To update the [[Python]] code.

Constraints:
- Inputs and outputs should stay the same for the time being.

There are two jobs, broken by region: `amer` and `emeaApac`.

# IO

## Inputs

### Platform metadata

There currently is a task responsible of providing two TSV files that contain metadata governing the definition of Lookalike segments:

- Pixels table: contains metadata on which pixel firers will be labelled as positives. `s3://af-tmp/lookalikes/latest/production_lookalike_pixels.tsv`
- Segments table: contains metadata related to how we sync the segments with DSP(s). `s3://af-tmp/lookalikes/latest/production_lookalike_segments.tsv`

### Enriched events

- First party
- Third party (one source per partner)

These are available in `af-reservoir` as output of the enricher service, and in the databricks table `production_metastore.events` as output of the `eventsPartitioner` job.

### Mapping events

- First party (`af`)
- Third party (`st`, `tt`)
- DSP party (`an`)

These are available in ` `, and in `production_metastore.mapping_{region}` as output of the `userIdsMapper` job.

## Outputs

### Segments

- Segments data as files in S3 for the `dsp-sync` job to pick them up and send them to our DSP. They currently consist of gzipped TSV files.

### Reports

- `supa_models_collection` table, in the [[Reporting|Reporting Redshift]] database, containing token weights for the corresponding models.
- Several other minor data reports: segment sizes, sizes of training datasets. 

# Tasks

## Currently

The codebase currently has the following PIG scripts, that should be updated to DAG steps (or similar).

1. `01_build_users_histories.pig`
2. `02_build_training_sets.pig`
3. `03_train.pig` -> `train.py`
4. `04_classify.pig` -> `classify.py`
5. `05_synch.pig`
6. `06_create_test_labels.should`
7. `07_evaluate.pig` -> `offline_evaluation.py`

## Proposal

We propose the following task outline:

### Platform metadata

We could simply recycle the existing task.

### Build training data

The purpose is to gather all necessary data from the sources and build training datasets that may be loaded by model selection tasks.

### Model selection

Select models, build and pickle them into storage.

### Scoring
Use trained models to score segment users.

### Segment sync

Send built segments to [[DSP]].

### (Optional) Report uploading