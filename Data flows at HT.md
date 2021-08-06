I intend to use this document to describe our tech stack and ways in which it can improve.

## Our data needs

A complex production [[ML|Machine Learning]] task typically involves three separate steps:

1. Data preprocessing
2. Model selection & training
3. Estimation broadcasting
4. Monitorization and validation

In some cases, only the first step is necessary. This is the case of aggregation jobs for reporting purposes, or very simple statistical tasks that do not involve any _active learning_. __Event repartition__ tasks are another great example where we only take the chance to change data format to make it faster to work with.

In some other cases, the nature of the model is such that training and estimation broadcasting can be performed at once. A good example is provided by __time-series decomposition__, where it doesn't pay to save a trained model away for further re-use.

Our current data needs fit this description, although there are a few technical constraints to consider: 

- __Databricks is our main data platform of choice__. We use it to manage our clusters and as a central piece in our data democratization strategy. We have few legacy jobs in EMR which we expect to either migrate to databricks or deprecate in the future.
- __Apache Airflow is our scheduler__. Although we are not fully satisfied about some of its internals, we adopt it as a lesser evil and have no plans to replace it by another scheduler.
- __All production code must be under version control__. 
- __We use TDD__. We aim for coverage of all production code. This includes unit testing, integration and end-to-end testing where necessary. Moreover, our testing also includes __a posteriori data validations__.
- __We have automatic deploys__ and systematically use __Continuous Integration__.

We intend to stick to these infrastructure choices and good practices in the present document and will not make proposals to amend them. In particular, while notebooks (jupyter/databricks) are a much beloved exploratory tool, we will not rely on them to run production jobs. 


## 1. Data preprocessing

This type of task involves loading data, filtering, aggregating and other preprocessing steps

