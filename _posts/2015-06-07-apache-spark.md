---
layout: post
title: "Technology Review : Apache Spark"
description: "Tech review for CS 6675"
category: [georgia tech]
tags: [big data, spark, recommendations]
---  

###Introduction

[Spark](http://spark.apache.org/) is a fast and general purpose engine built for large scale data processing. It was conceived and built at the AMPLab at University of California, Berkeley in 2009 by Matei Zaharia. Later in 2010, it was open sourced under a BSD license after which it was donated to the Apache Software Foundation in 2013. In 2014, Spark became an Apache Top Level Project. Spark is a new paradigm shift in big data and cluster computing in comparison to Hadoop.     


Hadoop architecture is mainly based on the two step MapReduce paradigm, which involves the splitting of files into large chunks of either 64 MB or 128 MB. These chunks are then distributed among nodes in the cluster. The Hadoop infrastructure takes advantage of data locality as well. The Hadoop framework is mainly written in the Java language and excepts the programmer to be aware of this programming paradigm for implementation of the MapReduce programs. The foundational model of MapReduce is based mainly on a distributed file system. The map task basically involves loading of the data and defining a set of keys for the same, whereas the reduce task involves collecting the organized key based data to process and produce output in the required format. The performance of Hadoop MapReduce program can be tweaked heavily based on the details of source files like the number of nodes, their sizes and other parameters.     


In contrast to the above implementation of two staged implementation of Hadoop, Spark is mainly based on in-memory storage and provides a performance of approximately 100 times better in comparison to Hadoop for certain kinds of applications. Spark mainly requires a cluster manager and a distributed storage system. Spark is known to outperform Hadoop by 10x in certain types of iterative machine learning tasks. It can also be used as a query interface to query a 39 GB database in sub second response time.    


Spark is mainly implemented in Scala, a statically typed high level programming language. It runs on Java’s JVM and is essentially a functional programming interface. It also has wrappers that allow developers to write programs in Java and Python as well. Each language has its own API exposed (i.e. Spark Java API for Java version and PySpark for the Python version) and these have their own differences and slight nuances when compared to the Scala API.



###Programming Model###


In order to use Spark, the developers need to write a driver program that implements
high level control flow of their application and launches parallel operations.
The major programming abstractions in case of Spark are:
<ul>
<li>Resilient Distributed Datasets (RDD)</li>
<li>Parallel Operations</li>
<li>Shared variables</li>
</ul>

#####Resilient Distributed Datasets (RDD)
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
