This document details a migration plan to upgrade the [[Lookalikes]] codebase from its current status to Databricks.

The development of the new pipeline is the goal of epic SAS-10960.

Particular changes that we are interested in:
- To move away from AWS [[EMR]] and into [[Databricks]].
- To move away code written in [[Apache Pig]] to [[Scala]] 2.X.
- To update the [[Python]] code, by taking advantage of [[scikit-learn]] enhancements.

Constraints:
- Inputs and outputs should stay the same for the time being.
- The behaviour of the models should be as close as possible to the existing ones.

There are two jobs, broken by region: `amer` and `emeaApac`.

__Note__: enhancements such as reading from metastore tables and updating the models will be proposed in a separate epic.

# IO

## Inputs

### Platform metadata

There currently is a task responsible of providing two TSV files that contain metadata governing the definition of Lookalike segments:

- Pixels table: contains metadata on which pixel firers will be labelled as positives. `s3://af-tmp/lookalikes/latest/production_lookalike_pixels.tsv`
- Segments table: contains metadata related to how we sync the segments with DSP(s). `s3://af-tmp/lookalikes/latest/production_lookalike_segments.tsv`

### Enriched events

- First party
- Third party (one source per partner)

These are available in `s3://af-reservoir/enriched-out/geo` as output of the enricher service, and in the databricks table `production_metastore.events` as output of the `eventsPartitioner` job.

### Mapping events

- First party (`af`)
- Third party (`st`, `tt`)
- DSP party (`an`)

These are available in `s3://af-mrdata/mapping/userIdsMapperIncremental/latest` or, equivalently, in `production_metastore.mapping_{region}`. In both cases, this is the output of the `userIdsMapper` job.

## Outputs

### Segments

Segments data as files in S3 for the `dsp-sync` job to pick them up and send them to our DSP. They currently consist of gzipped TSV files.

The output path is formatted out of the pattern: `s3://af-mrdata/segment_syncs/combined/2021/09/28/amer.lookalikeSegments.gz/`

__Schema__: TBD

### Reports

- `supa_models_collection` table, in the [[Reporting|Reporting Redshift]] database, containing token weights for the corresponding models.
- Several other minor data reports: segment sizes, sizes of training datasets. These will be made available as metastore tables. 

# Tasks

We propose the following task outline:

## Platform metadata

We could simply recycle the existing task.

__Output__: Two metadata files: `s3://af-tmp/lookalikes/latest/production_lookalike_{pixels,segments}.tsv` 

## Build training data

The purpose is to gather all necessary data from the sources and build training datasets that may be loaded by model selection tasks.

__Notebook guide__: [scala](https://dbc-d63cbbe9-c447.cloud.databricks.com/#notebook/42813/command/43062)
__Inputs__: s3 metadata, enriched events in json format, mapping tables.
__Outputs__: user histories, training data. They both can be saved as metastore tables, and partitioning may be considered.


## Model selection

Select models, build and pickle them into storage.

__Notebook__: [python](https://dbc-d63cbbe9-c447.cloud.databricks.com/#notebook/43429/command/43444)
__Inputs__: metadata, training data.
__Outputs__: registered, trained models in [[MLflow]], run ids and model selection metadata to `production_metastore.lookalikes_mlflow_runs`, token weights to `lookalikes_token_weights` (replacing `supa_models_collection`).

## Scoring

Use trained models to score segment users.

__Notebook__: [python](https://dbc-d63cbbe9-c447.cloud.databricks.com/#notebook/43603/command/43606)
__Inputs__: metadata, successful run information, registered models, user histories.
__Outputs__: scored user segments, as a metastore table.

## Segment sync

Adjust segment sizes and send built segments to the [[DSP]]. 

__Notebook__: [scala](https://dbc-d63cbbe9-c447.cloud.databricks.com/#notebook/43709/command/43738)
__Inputs__: scored segments, metadata, mapping events.
__Outputs__: Data to be picked by the `dsp-sync` job in `https://open.spotify.com/track/6RyuoOJXNzlVWpfC5xQyeI?si=0d9a81b69f834a5fto`.

## (Optional) Report uploading

TBD