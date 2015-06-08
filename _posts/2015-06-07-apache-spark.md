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

####Resilient Distributed Datasets (RDD)



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


####Parallel Operations


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

####Shared Variables



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




Example Applications using Spark
============




####Alternating Least Squares
<p>
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
</p>
