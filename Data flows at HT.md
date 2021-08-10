## TL;DR

Given the constraints of using [[Databricks]] as a main Data Platform and sticking to the Engineering Team's good practices, we propose to favour the following as a method to develop and deploy data pipelines:

0. Use __Airflow__ as a DAG scheduler
1. Data preprocessing: __Spark jobs__ written in __Scala__, deployed as JARs on DBFS and executed via Airflow's `DatabricksSubmitJobOperator`.
2. Model selection and training: __Python__ jobs executed on single-node Databricks clusters, via the same Airflow operator, and using __MLflow__ as a model registry.
3. Estimation broadcasting: __PySpark__ jobs capable of loading pickled models from the registry, registering a __pandas UDF__ and broadcasting the estimation to the cluster.

## Our data needs

A production data pipeline, especially when involving an [[ML]] task, requires performing the following steps:

1. Data preprocessing
2. Model selection & training
3. Estimation broadcasting
4. Monitorization and validation

In some cases, only the first step is necessary. This is the case of aggregation jobs for reporting purposes, or very simple statistical tasks that do not involve any _active learning_ (eg: computing an odds ratio). __Event repartition__ tasks are another great example where we only take the chance to change the data format to make it faster to work with.

In some other cases, the nature of the model is such that training and estimation broadcasting can be performed at once. A good example is provided by the __time-series decomposition__ used in __recurrent-analysis__, where it doesn't pay to save a trained model away for further re-use.

Our current data needs fit this description, although there are a few constraints: 

- __Databricks__ is our data platform of choice. 
- __Apache Airflow__ is our DAG scheduler.
- All __production code__ must be under __version control__. 
- __We stick to our team [values](https://hybridtheory.atlassian.net/wiki/spaces/EN/pages/1132036125/Engineering+Team+Values) and [good practices](https://hybridtheory.atlassian.net/wiki/spaces/EN/pages/5734406/Practices+Process)__. In particular, we use __TDD__ and __Continuous Integration__.

We intend to stick to these infrastructure choices and good practices in the present document and will not make proposals to amend them. In particular, while notebooks (jupyter/databricks) are a much beloved exploratory tool, we will not rely on them to run production jobs.

## 0. General architecture proposals

We will favour the organization of our pipelines as [[DAG]]s through [[Airflow]]. This will take advantage of the already existing infrastructure, an already in-place alert system and the ability to use other readily available operators.

### Cons

Airflow is cumbersome and involves writing a very specific flavour of [[Python]] to work. To the extent that at this point we have more Python code related to the configuration and deployment of [[DAG]]s than to [[ML]] models.

### Alternatives

- More recent versions of [Airflow](https://airflow.apache.org/) (nontrivial migration).
- [Luigi](https://luigi.readthedocs.io/en/stable/index.html). Very similar to Airflow.
- [Prefect](https://www.prefect.io/); it appeared more recently and it would be a serious candidate if we started from scratch.
- [Databricks jobs](https://databricks.com/blog/2021/07/13/announcement-orchestrating-multiple-tasks-with-databricks-jobs-public-preview.html) has recently added features to support the scheduling of DAGs. In this case, it is unclear how to leave a DAG definition as versioned code.
 
## 1. Data preprocessing

This type of task involves loading data, filtering, aggregating and other preprocessing steps. In other words, these are [[SQL]] operations.

The volumes of data that we manipulate require significant parallelization efforts and we intend to rely on [[Spark|Apache Spark]] for this purpose; this is in accordance with choosing [[Databricks]] as a main data platform.

Spark is a standalone system, originally written in [[Scala]] and running on top of the [[JVM]]. This is a good match, since Scala has many properties as a programming language that are desirable for coding robust data pipelines: it is functional, provides immutability of values, has a strong type system and lazy evaluation. Moreover, it is a compiled language, which means that code can be packaged and distributed to a cluster easily.

For this matter, writing [[Scala]] code and packaging it into a [[JAR]] is our tool of choice for data processing tasks. After copying the [[JAR]] to a cloud storage location (S3, DBFS), [[Airflow]] tasks can be easily set up to run this JAR on a [[Spark]] cluster using the [DatabricksSubmitRunOperator](https://airflow.apache.org/docs/apache-airflow-providers-databricks/stable/operators.html). 

We should remark that this approach is currently the favoured one in most of the existing data engineering pipelines: `events-partitioner`, `user-ids-mapper`, `url-audiences`.
 
### Legacy tools

We currently still have data pipelines running in the following infrastructures.

- Launching [[Apache Pig]] jobs on [[EMR]]. Although some pipelines currently exist using this technology, they are regarded as legacy and mainteinance efforts are carefully assessed. [[Lookalikes]] and [[Domain Clusters]] are two good examples.
- Launching [[Spark]] jobs on [[EMR]] via [[JAR]]. Migration from [[EMR]] to [[Databricks]] is relatively simple and only some legacy tasks related to the [[Brain]] models currently fit in this pattern.
- Aggregating data on [[Redshift]] and then dumping it to S3. Redshift has been used as a main [[Data Warehouse]] before migrating to [[Databricks]]'s own [[Delta Lake]]. In this case, jobs were launched as [[SQL]] scripts via the `RedshiftOperator` in [[Airflow]] and the computation takes place on the [[Redshift Computing]] cluster. The [[Category Audiences]] job matches this pattern.

### Cons

Coding in Scala presents some drawbacks when compared to other available options. In particular, setting up a project using [[SBT]] to run a [[Spark]] job involves quite an amount of boilerplate. This is, however, a problem that we have already solved, since we have multiple jobs already implemented and we have extracted templates from them.

It is hard to find [[Scala]] developers in the job market, especially compared to [[Python]] or [[SQL]]. However, it is quite a staple in the CVs of mid/senior Data Engineers.

Coding in Scala is harder, but the result is of better quality. An ETL Spark job would probably not present many differences in [[Scala]] and [[Python]] in terms of code, but there are many advantages to being able to use Spark implicits and compilation errors.

### Alternatives

- We have our own [[Airflow]] operator capable of connecting to a [[Databricks]] cluster via [[JDBC]] and executing a [[SQL]] script. We discard using this approach because [[SQL]] as a language has very little capability for abstraction. Eg: we use [[Jinja]] templating and unit tests that check templated strings to be able to distinguish between our production and alpha environments.
- Using [[PySpark]] jobs. We would have a very similar way of interacting with [[Spark]]. However, since [[Python]] is an interpreted language, packaging code and distributing it across nodes in a cluster is a funny businees, which involves zipping code, pickling and playing around the [[GIL]].

## 2. Model selection & training

In some cases, we desire to use [[ML]] as part of the data pipeline. More precisely, it will be desirable to invoke the usage of a framework capable of downloading trained models, training its own models and storing them away. This framework is most often a [[Python]] package:

- [[scikit-learn]]. Currently in use in the [[Lookalikes]] pipeline.
- [[Gensim]]. Currently used to train [[Word2Vec]] embeddings in the [[Domain Clusters]] pipeline.
- [[TensorFlow]], [[Stanza]], [[NLTK]] have been used for prototypes but not deployed to production as of yet.

By the time we get to the stage of selecting and training a model, data has typically been sufficiently aggregated to the point of fitting into a single machine. Moreover, the parallelization problems posed by training and selecting a model are slightly different than those described in [[#1 Data preprocessing]]. Many [[ML]] algorithms run on a single core. With few exceptions, parallelization happens at a model selection level: this is the case of `GridSearchCV` from [[scikit-learn]], which we use profusely.

For this reason, we intend to use the single-node clusters provided by [[Databricks]]. Significant advances have been made recently, and the most relevant one is the [ML Runtime](https://docs.databricks.com/release-notes/runtime/8.4ml.html), which includes a distribution of most commonly used [[ML]] packages.

The [[Databricks]] jobs API will run python scripts by deploying them to cloud storage ([[S3]] or [[DBFS]]) and passing the path in the call to the API. In practice, this means that we will run a [[Python]] script by passing a path to a [[DBFS]] location in the `DatabricksSubmitRunOperator` Airflow call.

Moreover, we strongly recommend adopting [[MLflow]] as tool to track model performance and to act as a model registry. Access to MLflow is automatically granted anywhere within the Databricks environment (thus making interaction with Notebooks transparent), or from any system that has the `databricks-cli` Python package configured.

Regarding the eventual problem of parallelization of model selection tasks, the key issue will be leveraging the usage of a Spark cluster as a parallel backend in replacement of [[scikit-learn]]'s default `joblib`. [[Dask]], [[Koalas]] (shipping with the ML Runtime) and [spark-sklearn](https://databricks.com/blog/2016/02/08/auto-scaling-scikit-learn-with-apache-spark.html) should be helpful in that direction; this is an area subject to a lot of research in recent years.

### Cons

Single-node Databricks clusters are a bit more expensive when compared to other options. We suggest to carry out a cost-assessment.

### Alternatives

- A very convenient way to run Python code is to package it into a [[Docker]] container. Although we have a few examples of such artifacts in production (`docker-airflow`, `recurrent-analysis`, `url-scraper`), this raises the question of where to execute these containers: EC2, AWS Fargate, a Kubernetes cluster, etc. Given the current [[ML]] needs, this type of solution feels over-engineered.
- We could look into using [[spark-ml]] for training Spark-native, machine learning models directly in [[Scala]]. This solution is not as widely adopted as the suggested one and presents problems of its own, namely the performance of the available models and the limited availability of implemented models.

## 3. Model propagation

Once a model is trained, we need to load it and compute predictions, scores and/or decision probabilities.

This step should use the same infrastructure as that of [[#2 Model selection training]], for the simple reason that we will have a model already trained and saved away that may only be loaded under similar circumstances.

Given our choices, this means running a [[Python]] script that loads a pickled model and calls one of its estimation methods.

The preferred way to perform this job is to put the call to the estimation method inside of a UDF (especially if it may be a pandas UDF) and broadcasting it to a spark cluster.

Hence, this task should very clearly consist of the following steps:
- Loading a pickled model by calling the [[MLflow]] registry.
- Registering a pandas UDF in a spark cluster that calls the model's estimation method.
- Computing predictions in parallel using [[Spark]]'s `DataFrame.withColumn()`.

### Cons

As described in [[#1 Data preprocessing]], we might run into problems with [[Python]]'s __GIL__ because the method defining the UDF must be picklable.

However, predict methods on libraries such as [[scikit-learn]] and [[TensorFlow]] are carefully designed to be picklable and we should be fine provided we do not perform much fine-tuning on top of them.

[[MLflow]] comes in handy in this task, as models pickled in the registry are made easily available. A code snippet to invoke a model would look as follows:

```python
import mlflow

# This is a string uniquely identifying the model.
# It is the only information needed for recovery.
logged_model = 'runs:/ba234e99f7914a19b02ba5f34cbe0357/best_estimator'

# Load model as a Spark UDF.
loaded_model = mlflow.pyfunc.spark_udf(spark, model_uri=logged_model)

# Predict on a Spark DataFrame.
df.withColumn('predictions', loaded_model(*columns)).collect()
```

## 4. Monitorization and Validation

We regard validating our data pipelines as an integral part of our systems. In particular:

- We should be able to easily access scoring metrics after model selection and training. We would rely on the [[MLflow]] tracking capabilities for this.
- We should run a posteriori validations of supervised algorithms, particularly if they involve forecasting.
- Optionally, we should receive push notifications (email/Slack) when validation metrics drop below established thresholds.
