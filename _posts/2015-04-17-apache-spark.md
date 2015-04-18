---
layout: post
title: "Technology Review : Apache Spark"
description: "Tech review for CS 6675"
category: [georgia tech]
tags: [big data, spark, recommendations]
---  


[Spark](http://spark.apache.org/)) is a fast and general purpose engine built for large scale data processing. It was conceived and built at the AMPLab at University of California, Berkeley in 2009 by Matei Zaharia. Later in 2010, it was open sourced under a BSD license after which it was donated to the Apache Software Foundation in 2013. In 2014, Spark became an Apache Top Level Project. Spark is a new paradigm shift in big data and cluster computing in comparison to Hadoop.     


Hadoop architecture is mainly based on the two step MapReduce paradigm, which involves the splitting of files into large chunks of either 64 MB or 128 MB. These chunks are then distributed among nodes in the cluster. The Hadoop infrastructure takes advantage of data locality as well. The Hadoop framework is mainly written in the Java language and excepts the programmer to be aware of this programming paradigm for implementation of the MapReduce programs. The foundational model of MapReduce is based mainly on a distributed file system. The map task basically involves loading of the data and defining a set of keys for the same, whereas the reduce task involves collecting the organized key based data to process and produce output in the required format. The performance of Hadoop MapReduce program can be tweaked heavily based on the details of source files like the number of nodes, their sizes and other parameters.     


In contrast to the above implementation of two staged implementation of Hadoop, Spark is mainly based on in-memory storage and provides a performance of approximately 100 times better in comparison to Hadoop for certain kinds of applications. Spark mainly requires a cluster manager and a distributed storage system. Spark is known to outperform Hadoop by 10x in certain types of iterative machine learning tasks. It can also be used as a query interface to query a 39 GB database in sub second response time.    


Spark is mainly implemented in Scala, a statically typed high level programming language. It runs on Java’s JVM and is essentially a functional programming interface. It also has wrappers that allow developers to write programs in Java and Python as well. Each language has its own API exposed (i.e. Spark Java API for Java version and PySpark for the Python version) and these have their own differences and slight nuances when compared to the Scala API.
