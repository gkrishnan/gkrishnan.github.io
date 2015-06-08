---
layout: post
title: "Technology Review : Apache Spark"
description: "Tech review for CS 6675"
category: [georgia tech]
tags: [big data, spark, recommendations]
---  

Introduction
============

[Spark](http://spark.apache.org/) is a fast and general purpose engine built for large scale data processing. It was conceived and built at the AMPLab at University of California, Berkeley in 2009 by Matei Zaharia. Later in 2010, it was open sourced under a BSD license after which it was donated to the Apache Software Foundation in 2013. In 2014, Spark became an Apache Top Level Project. Spark is a new paradigm shift in big data and cluster computing in comparison to Hadoop.     


Hadoop architecture is mainly based on the two step MapReduce paradigm, which involves the splitting of files into large chunks of either 64 MB or 128 MB. These chunks are then distributed among nodes in the cluster. The Hadoop infrastructure takes advantage of data locality as well. The Hadoop framework is mainly written in the Java language and excepts the programmer to be aware of this programming paradigm for implementation of the MapReduce programs. The foundational model of MapReduce is based mainly on a distributed file system. The map task basically involves loading of the data and defining a set of keys for the same, whereas the reduce task involves collecting the organized key based data to process and produce output in the required format. The performance of Hadoop MapReduce program can be tweaked heavily based on the details of source files like the number of nodes, their sizes and other parameters.     


In contrast to the above implementation of two staged implementation of Hadoop, Spark is mainly based on in-memory storage and provides a performance of approximately 100 times better in comparison to Hadoop for certain kinds of applications. Spark mainly requires a cluster manager and a distributed storage system. Spark is known to outperform Hadoop by 10x in certain types of iterative machine learning tasks. It can also be used as a query interface to query a 39 GB database in sub second response time.    


Spark is mainly implemented in Scala, a statically typed high level programming language. It runs on Java’s JVM and is essentially a functional programming interface. It also has wrappers that allow developers to write programs in Java and Python as well. Each language has its own API exposed (i.e. Spark Java API for Java version and PySpark for the Python version) and these have their own differences and slight nuances when compared to the Scala API.



Programming Model
=================


In order to use Spark, the developers need to write a driver program that implements
high level control flow of their application and launches parallel operations.
The major programming abstractions in case of Spark are:
<ul>
<li>Resilient Distributed Datasets (RDD)</li>
<li>Parallel Operations</li>
<li>Shared variables</li>
</ul>

Resilient Distributed Datasets (RDD)
-------

An RDD is a read-only collection of objects. These objects are partitioned
across a set of machines and hence these can be rebuilt in the event a partition
is lost. The elements of the RDDs need not exist in physical storage. It is
designed in such a way that a handle to an RDD contains enough information
to compute the RDD starting from the data in reliable storage. In this manner,
RDDs can always be reconstructed in case of any failures. In the Spark scenario,
each RDD is represented as a Scala object. There are four ways to construct an
RDD, which are described as follows:
<ol>
<li>Construction from a file: A new RDD can be constructed from an
existing file in a shared file system (HDFS).</li>
<li> Parallelizing a Scala collection: A new RDD can also be constructed
by parallelizing a Scala collection in the driver program. This essentially
means dividing a collection into multiple slices that would be sent to multiple
nodes.</li>
<li> Transforming an already existing RDD: This involves transformation
of a dataset with elements of type A into a dataset with elements of type
B. This can be achieved in many ways, one of which would be by using
an operation called flatMap, which passes each element through a userprovided
function of type A =⇒ List[B]. Other transformations like map
and filter can be easily expressed using the flatMap operation.</li>
<li> Changing the persistence of an existing RDD: RDDs are lazy and
ephemeral by default behavior. The partitions of a dataset are materialized
only on demand when they are used in a parallel operation, for
instance passing a block of a file through a map function. These are discarded
from memory after use. But, a user can alter the persistence of an
RDD through two actions:
<ul>
<li> The cache action leaves the dataset lazy, but hints that it should be
kept in memory after the first time it is computed, because it will be
reused.</li>
<li> The save action evaluates the dataset and writes it to a distributed
filesystem such as HDFS. The saved version is used in future operations
on it.</li></ul>
</li></ol>


Parallel Operations
-------

<ul>
Many parallel operations can be performed on RDDs, including:
<li> Reduce: Combines the dataset elements using an associative function to
produce a result at the driver program.</li>
<li> Collect: Send all elements of the dataset to the driver program. For
instance, an easy way to update an array in parallel is to parallelize, map
and collect the array.</li>
<li> Foreach : Passes each element through a user provided function. This is
only done for the side effects of the function, which may be copying data
to another system or to update a shared variable.</li>
</ul>

Shared Variables
-------


It is a common scenario that programmers use operation such as map, filter
and reduce by passing functions to Spark. These functions usually refer to the
variables in scope where they have been created. In order to avoid incessant
copies of the variables and related hassles, Spark allows programmers to create
two restricted types of shared variables, namely broadcast variables and
accumulators.

#####Broadcast Variables


If a large read-only piece of data is used in multiple parallel operations, it is
preferable to distribute it to the workers only once instead of packaging it with
every closure. Spark lets the programmer create a “broadcast variable” object
that wraps the value and ensures that it is only copied to each worker once.


#####Accumulators


These are variables that workers can only “add” to using an associative operation
and that only the driver can read. They can be used to implement
counters as in MapReduce and to provide a more imperative syntax for parallel
sums. Accumulators can be defined for any type that has an “add” operation
and a “zero” value. Due to their “add-only” semantics, they are easy to make
fault-tolerant

 Building and Running Spark Applications
============


Spark Shell
-------

Spark Shell is a command line interface for running Spark commands. It is
a very useful tool for rapid prototyping with Spark. The Spark Shell allows
interaction and querying of the Spark cluster. It is useful while debugging
complex Spark programs and in general as a test bed for all experiments. Many
utility commands can be run using the Spark Shell ranging from loading text
files into Resilient Distributed Datasets (RDDs), which has been discussed in
the earlier sections of this technology review. The Spark Shell is based on the
Scala REPL (Scala interactive shell) and hence it inherits all the power of REPL
(Read-Evaluate-Print-Loop).

Scala Build Tool
-------


The Scala Built Tool (SBT) is a very popular build tool for Scala that supports
both Scala and Java code. Building Spark projects with sbt is one of the easiest
options because Spark itself is built with sbt. It makes it easy to bring in
dependencies, which are very helpful for Spark. SBT also helps in packaging everything
into a single deployable/JAR file. Since the JAR file would be shipped
to the other machines, the developer must ensure that all the dependencies are
included in them. To achieve this, either a bunch of JAR files could be added
or an sbt plugin called sbt-assembly could be used in order to group everything
into a single JAR file. If the dependencies are not transient, the developer can
opt to skip using the plugin and instead could choose to run sbt/sbt assembly
task in the Spark project and then add the resulting JAR file core/target/sparkcore-assembly-0.X.0.jar
to the classpath. The sbt-assembly package is a great
tool to avoid having to manually manage a large number of JAR files.

Spark Modules
============


<img src="/assets/stack.png" alt="Spark Stack" style="width: 400px;"/>


There are many popular modules which work on top of Spark as shown in Figure
1 above. Few of the prominent ones are listed and explained below:
<ul>
<li>Spark SQL</li>
<li>MLlib</li>
<li>GraphX</li>
<li>Spark Streaming</li>
</ul>



Spark SQL
---------


Spark SQL is a Spark module for structured data processing. It provides a
programming abstraction called DataFrames and can also act as distributed
SQL query engine.


#####DataFrames
A DataFrame is a distributed collection of data organized into named columns.
It is conceptually equivalent to a table in a relational database or a data frame
in R/Python, but with richer optimizations under the hood. DataFrames can
be constructed from a wide array of sources such as: structured data files, tables
in Hive, external databases, or existing RDDs.The DataFrame API is available
in Scala, Java, and Python.


Spark SQL lets developers query structured data as a distributed dataset (RDD)
in Spark, with integrated APIs in Python, Scala and Java. This tight integration
makes it easy to run SQL queries alongside complex analytic algorithms.
SparkSQL allows unified data access, which means that it can accept data from
multiple sources seamlessly. SparkSQL is compatible with Hive, which means
that Hive queries can be run on existing warehouses without any modification.
Spark SQL can use existing Hive metastores, Serializer Desearializer(SerDes)
and user defined funcations (UDFs) as shown in Figure 2.


<img src="/assets/sql_hive.png" alt="Spark Stack" style="width: 400px;"/>


MLlib
--------


MLlib is Spark’s scalable machine learning library consisting of common learning
algorithms and utilities, including classification, regression, clustering, collaborative
filtering, dimensionality reduction and underlying optimization primitives.
Some of the prominent algorithms available in MLlib 1.1 are:
<ul>
<li>Linear SVM and logistic regression</li>
<li>Classification and regression tree</li>
<li>K-means clustering</li>
<li>Recommendation via alternating least squares</li>
<li>Singular Value Decomposition</li>
<li>Linear regression with L1- and L2-regularization</li>
<li>Multinomial naive Bayes</li>
<li>Basic statistics</li>
<li>Feature transformations</li>
</ul>


GraphX
------------

GraphX is a new component in Spark for graphs and graph-parallel computation.
GraphX extends the Spark RDD by introducing a new Graph abstraction:
a directed multigraph with properties attached to each vertex and edge. To support
graph computation, GraphX exposes a set of fundamental operators like
subgraph, joinVertices, and aggregateMessages as well as an optimized variant
of the Pregel API. In addition, GraphX includes a growing collection of graph
algorithms and builders to simplify graph analytics tasks.



#####Property Graph


The property graph is a directed multigraph with user defined objects attached
to each vertex and edge. A directed multigraph is a directed graph with potentially
multiple parallel edges sharing the same source and destination vertex.
The ability to support parallel edges simplifies modeling scenarios where there
can be multiple relationships (e.g., co-worker and friend) between the same vertices.
Each vertex is keyed by a unique 64-bit long identifier (VertexID). GraphX
does not impose any ordering constraints on the vertex identifiers. Similarly,
edges have corresponding source and destination vertex identifiers.


The property graph is parameterized over the vertex (VD) and edge (ED)
types. These are the types of the objects associated with each vertex and edge
respectively.


#####Example Property Graph
Suppose we want to construct a property graph consisting of the various collaborators
on the GraphX project. The vertex property might contain the username
and occupation. We could annotate edges with a string describing the relationships
between collaborators as shown in Figure 3

<img src="/assets/propertygraph.png" alt="Example Property Graph" style="width: 400px;"/>


Spark Streaming
----------

Spark Streaming is an extension of the core Spark API that enables scalable,
high-throughput, fault-tolerant stream processing of live data streams. Data can
be ingested from many sources like Kafka, Flume, Twitter, ZeroMQ, Kinesis or
TCP sockets can be processed using complex algorithms expressed with highlevel
functions like map, reduce, join and window. Finally, processed data can
be pushed out to filesystems, databases, and live dashboards. Streaming can be
used to implement Spark’s machine learning and graph processing algorithms
on data streams. The architecture is as shown in Figure 4:


<img src="/assets/streaming.png" alt="Spark Streaming" style="width: 400px;"/>


Internally, it works as follows. Spark Streaming receives live input data
streams and divides the data into batches, which are then processed by the
Spark engine to generate the final stream of results in batches. Spark Streaming
provides a high-level abstraction called discretized stream or DStream, which
represents a continuous stream of data as shown in Figure 5 below. DStreams can be
created either from input data streams from sources such as Kafka, Flume, and
Kinesis, or by applying high-level operations on other DStreams. Internally, a
DStream is represented as a sequence of RDDs.


<img src="/assets/streamingflow.png" alt="Spark Streaming" style="width: 600px;"/>


Spark on Amazon Web Services
============


Spark is often run on Amazon Web Services Platform with the help of it’s EC2
(Elastic Compute Cloud) machines or on the EMR (Elastic Map Reduce) service.
Considering the flexibility offered by the cloud services, this is a very
prominent use case. Usage of cloud based services also aids in the flexibility
and easy addition of multiple instances or new machines in case the processing
power is not enough for the set of machines that have currently been provisioned
for this purpose. Detailed configuration can be carried out on AWS EC2
in order to derive the full benefit of Spark. The Spark team provides special
EC2 AMIs (Amazon Machine Images) that can be utilized while running your
Spark implementation on AWS. This may be tailored to a particular build or
version of Spark, so these AMIs may not work for your specific use case if there
is any custom modifications have been made to Spark.



The alternate option is to use Amazon’s Elastic Map Reduce (EMR) service.
Amazon EMR is a web service that makes it easy to run Hadoop clusters using
AWS resources such as Amazon EC2 virtual server instances. The enables the
developer to launch clusters without having to maintain a data center of physical
hardware. Amazon also follows pay per usage model, where in a flat fee does
not need to be paid for the service, but only as per the developer’s usage. This is
especially useful for applications that use the Spark framework. Spark stores all
the data in memory due to which its CPU and memory requirements can vary
depending on the workload. The developer can change the size of the cluster
while running the EMR job as per the application’s needs. EMR also allows
a change to the number of nodes in a cluster as Spark’s resource requirements
change.


Figure 6 below illustrates running Spark on a Hadoop cluster managed by Amazon EMR. When a cluster is launched, Amazon EMR provisions virtual server instances from Amazon EC2. These can be used as an Amazon EMR bootstrap
action to install Spark on the cluster. Spark on Amazon EMR can process data
from an Amazon S3 (Simple Storage Service) bucket or stored in HDFS, and
the results from these queries can also be persisted back to Amazon S3 or HDFS
after analysis.


<img src="/assets/AWS_spark.png" alt="Spark on AWS" style="width: 600px;"/>




Example Applications using Spark:
============




Alternating Least Squares
------------
A good example top demonstrate the usability of Spark is alternating least
squares (ALS) algorithm. ALS is used for collaborative filtering problems, such
as predicting users’ ratings for movies that they have not seen based on their
movie rating history. The ALS algorithm is known to be CPU-intensive.
Suppose that we wanted to predict the ratings of u users for m movies, and
that we had a partially filled matrix R containing the known ratings for some
user-movie pairs. ALS models R as the product of two matrices M and U of
dimensions m ∗ k and k ∗ u respectively. This means that each user and each
movie has a k-dimensional “feature vector” describing its characteristics, and a
user’s rating for a movie is the dot product of its feature vector and the movie’s.
ALS solves for M and U using the known ratings and then computes M ∗ U to
predict the unknown ones. This is done using the following iterative process:


<ol>
<li>Initialize M to a random value.</li>
<li>Optimize U given M to minimize error on R.</li>
<li>Optimize M given U to minimize error on R.</li>
<li>Repeat steps 2 and 3 until convergence.</li>
</ol>
<br/><br/>
ALS can be parallelized by updating different users or movies on each node in
steps 2 and 3. However, because all of the steps use R, it is helpful to make R
a broadcast variable so that it does not get resent to each node on every step.


It could be observed that without using broadcast variables, the time to resend
the ratings matrix R on each iteration dominated the job’s running time.
Furthermore, with a naive implementation of broadcast (using HDFS or NFS),
the broadcast time grew linearly with the number of nodes, limiting the scalability
of the job. This was implemented using an application-level multicast
system to mitigate this issue. But, in spite of a fast broadcast, resending R on
each iteration is costly. Caching R in memory on the workers using a broadcast
variable improved performance by 2.8x in an experiment with 5000 movies and
15000 users on a 30-node EC2 cluster.
