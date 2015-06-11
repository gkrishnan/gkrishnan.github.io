---
layout: post
title: "Introduction to Apache Spark - Part 2"
description: "Intro to Spark"
category: [bigdata]
tags: [spark, shell, sbt]
---  


Parts
==============

This post is the second among a multi part series about an introduction to spark and how to build, run a Spark Hello World Application. Here is an index of the posts:


[Part 1 - Intro and Programming Model](http://www.gkrishnan.com/bigdata/2015/06/07/apache-spark-part1-intro/) 


Part 2 - Building and running Spark applications - - This post


[Part 3 - Coding up a Hello World application on Spark](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part3-hello-world/)


[Part 4 - Spark Modules](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part4-spark-modules/)


[Part 5 -Spark on AWS and Alternating Least Squares](http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part5-spark-aws/)

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
tool to avoid having to manually manage a large number of JAR files. The next section *'Hello World in Spark'* makes use of the sbt to create a jar file that can then be submitted to spark for processing.


Hello World in Spark
============


I have written up a separate post about how to set up a simple Hello World application in Spark which detects the total number of lines that contain the word 'error' in a logfile here: <http://www.gkrishnan.com/bigdata/2015/06/09/apache-spark-part3-hello-world> 



References
=============

<ol>
<li>Spark official documentation page: https://spark.apache.org/documentation.html</li>
</ol>
<br/>

