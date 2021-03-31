# Apache Spark

## Download Spark
Local build:

Date: March 2021

Spark release 3.1.1; package type "prebuilt for Hadoop 2.7"; openjdk 15.0.2; python 3.9.2 and pyspark;

## PySpark Shell

To start PySpark on Linux/OS X

```
$HOME/spark-3.1.1-bin-hadoop2.7/bin/pyspark
```

or install with pip and start pyspark

```
pip install pyspark
pyspark
```

# theory
Reference: Spark in Action Second Edition, Jean-Georges Perrin, Manning Publications.

## the big picture 
- Apache Spark 3 is certified to run on Java 11 [ref: book 1.5.1]
- Supports APIs for Scala, R, Python, Java.

### what is it and what is it used as
  - spark can be imagined as an analytics operating system.
  - automatically manages the distrubuted nodes under it.
  - provides standardised spark SQL for rdbms.
  - to utilize spark, it is not necessary to understand the workings under the hood.
  - refer: figure 1.3;
  - excels in big data scenarios: data eng, data sciences, etc.
  - Basic steps: Ingestion (raw data), Improvement of data quality (pure data), Transformation (rich data), publication. fig 1.5

### four pillars of spark
  - ref: fig 1.4
  - Spark SQL: data operations, offers API and SQL
  - Spark streaming (uses RDD), Structured streaming(uses Spark SQL and dataframe): to analyze streaming data.
  - Spark MLlib: for ML, DL
  - Spark GraphX: to manipulate graph-based data structures within spark.

### storage and APIs; the dataframe
  - essential to spark, dataframe. A data container and an API.
  - fig 1.7;
  - API is extensible through user defined functions (UDF), [ref ch 16]

## architecture and flow
- the application is the driver. 
- data can be processed and manipulated within the master, even remotely.

### typical flow of data in an app
  - ref fig 2.9
  - Connecting to a master: for every spark app, first operation is to connect to the spark master and get a spark "session."
    - master can be either local or remote cluster.
  - **Ingestion**: ask spark to load the data
    - spark relies on the distrubuted worker nodes to do the work.
    - the allocated workers ingest at the same time.
    - the filesystem must be of either shared format or of a distrubuted filesystem.
    - each worker node assigns partitions in memory for ingestion and creates tasks which ingests the records from the filesystem/file.
    - data is ingested by all assigned worker nodes simultaneosly.
  - **Transformation**: tasks within each node will perform the transformations
    - entire processing takes place within the workers.
  - **Saving or Publishing**: the processed data is stored in the database or published from the partitions.
    - if there is four partitions then saving to database will require four connections to the database. 
    - if there is 200,000 tasks then it causes 200,000 simultaneos connection to the database.
    - chapter 17: solution to the load issue.

## role of the dataframe

- dataframe: data structure similar to a table in the relational database world.
  - **immutable** distrubuted collection of data.
  - organized into **columns**.
  - a dataframe is an RDD with a schema.
  - use dataframe over an RDD when **performance is critical**.
- transformations: operations performed on data.
- resilient distrubuted dataset (RDD): dataframe is built on top of the RDD concept. RDD was the first generation of storage in Spark.
- **APIs across Spark libs are unified under the dataframe API**.

### using dataframe in python
[pyspark.sql API](https://spark.apache.org/docs/latest/api/python/reference/pyspark.sql.html)

### essential role of dataframe
  - figure 3.1

### data immutability
  - **Spark stores the initial state of the data, in an immutable way, and then keeps the recipe (a list of transformations.)**
  - The access to data is kept in sync with the nodes.
  - Spark stores only the steps of the transformation.
  - This becomes intuitive when the nodes are distrubuted.
  - fig 3.4

### Catalyst 

- Spark builds the list of transformations as a directed acyclic graph (DAG)
- DAG is optimized by Catalyst, built-in optimizer.

# Ingestion

## from files
  - regex can be used to specify the path
  - to parse CSV files, spark provides a rich set of options to tune the parser.
  - Spark can ingest both one-line and multiline JSON
  - To ingest XML, a plugin provided by Databricks is required.
  - Avro, ORC and Parquet are well supported by Spark. 
  - The pattern for ingesting is fairly similar: specify the format and read.

## from databases
  - Spark requires JDBC drivers.
  - Spark comes with support for IBM Db2, Apache Derby, MySQL, Microsoft SQL Server, Oracle, PostgreSQL and Terradata Database.
  - Joins can be performed at database level prior to ingestion, but can also do joins in Spark.
  - Ingesting data from Elasticsearch follows same principle as any other ingestion.
  - Elasticsearch contains JSON documents, which are ingested as is in Spark.

## advanced
  - WIP

## from streaming data
  - WIP

# Transformation

## SQL

## data

## entire documents

## UDF

## Aggregating








