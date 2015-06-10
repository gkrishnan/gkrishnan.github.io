---
layout: post
title: "Introduction to Apache Spark"
description: "Intro to Spark"
category: [bigdata]
tags: [big data, spark, intro, programming model]
---  

This post is the first among a multi part series about an introduction to spark and how to build, run a Spark Hello World Application. Here is an index of the posts:


Part 1 - Intro and Programming Model - This post


[Part 2 - Building and running Spark applications](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part2-building-running-apps/)


[Part 3 - Coding up a Hello World application on Spark](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part3-hello-world/)


[Part 4 - Spark Modules](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part4-spark-modules/)


[Part 5 -Spark on AWS and Alternating Least Squares](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part5-spark-aws/)





Introduction
============

[Spark](http://spark.apache.org/) is a fast and general purpose engine built for large scale data processing. It was conceived and built at the AMPLab at University of California, Berkeley in 2009 by Matei Zaharia. Later in 2010, it was open sourced under a BSD license after which it was donated to the Apache Software Foundation in 2013. In 2014, Spark became an Apache Top Level Project. Spark is a new paradigm shift in big data and cluster computing in comparison to Hadoop.     


Hadoop architecture is mainly based on the two step MapReduce paradigm, which involves the splitting of files into large chunks of either 64 MB or 128 MB. These chunks are then distributed among nodes in the cluster. The Hadoop infrastructure takes advantage of data locality as well. The Hadoop framework is mainly written in the Java language and excepts the programmer to be aware of this programming paradigm for implementation of the MapReduce programs. The foundational model of MapReduce is based mainly on a distributed file system. The map task basically involves loading of the data and defining a set of keys for the same, whereas the reduce task involves collecting the organized key based data to process and produce output in the required format. The performance of Hadoop MapReduce program can be tweaked heavily based on the details of source files like the number of nodes, their sizes and other parameters. The above was a very high level overview of Hadoop MapReduce, but if you wanted to do a deep dive into Hadoop/MapReduce, here are a few links that I found useful :


[http://stevekrenzel.com/finding-friends-with-mapreduce](http://stevekrenzel.com/finding-friends-with-mapreduce) : This blog talks about the application of MapReduce to determine mutual friends on Facebook


[http://stackoverflow.com/questions/12375761/good-map-reduce-examples-for-explanation](http://stackoverflow.com/questions/12375761/good-map-reduce-examples-for-explanation): This is a good resource for example applications of MapReduce


[http://research.google.com/archive/mapreduce.html](http://research.google.com/archive/mapreduce.html): This is a link to the seminal MapReduce Paper written by Jeffrey Dean and Sanjay Ghemawat



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
RDD through two actions:</li></ol>
<ul>
<li> The cache action leaves the dataset lazy, but hints that it should be
kept in memory after the first time it is computed, because it will be
reused.</li>
<li> The save action evaluates the dataset and writes it to a distributed
filesystem such as HDFS. The saved version is used in future operations
on it.</li></ul>



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
that wraps the value and ensures that it is only copied to each worker once. Broadcast variables 
permit the programmer to create read only variable cached on a machine, rather than shipping copies
of it to every machine with the tasks. Also, once a broadcast object is created, that should be used
over the actual variable to ensure that the broadcast functionality is being correctly utilized. Another
important item to be noted is that the broadcast object should not be modified, to ensure that all the 
tasks get the correct values of the broadcast objects.


#####Accumulators


These are variables that workers can only “add” to using an associative operation
and that only the driver can read. They can be used to implement
counters as in MapReduce and to provide a more imperative syntax for parallel
sums. Accumulators can be defined for any type that has an “add” operation
and a “zero” value. Due to their “add-only” semantics, they are easy to make
fault-tolerant. The accumulators are also displayed in the SparkUI, if created 
with a name and hence, make it easy for the developers to keep track of progress 
in their application.


References
=============

<ol>
<li>Zaharia, Matei, et al. "Spark: Cluster computing with working sets." Proceedings
of the 2nd USENIX conference on Hot topics in cloud computing.
2010.</li>
<li>Karau, Holden. Fast Data Processing With Spark. Packt Publishing Ltd,
2013.</li>
<li>Zaharia, Matei, et al. "Resilient distributed datasets: A fault-tolerant
abstraction for in-memory cluster computing." Proceedings of the 9th
USENIX conference on Networked Systems Design and Implementation.
USENIX Association, 2012.</li>
<li>Spark official documentation page: https://spark.apache.org/documentation.html</li>
</ol>
<br/>

