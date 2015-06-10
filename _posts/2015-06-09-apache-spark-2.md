---
layout: post
title: "Introduction to Apache Spark - Part 2"
description: "Intro to Spark"
category: [bigdata]
tags: [big data, spark, recommendations]
---  


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


I have written up a separate post about how to set up a simple Hello World application in Spark which detects the total number of lines that contain the word 'error' in a logfile here: <http://www.gkrishnan.com/bigdata/2015/06/09/spark-hello-world/> 



References
=============

<ol>
<li>Zaharia, Matei, et al. "Spark: Cluster computing with working sets." Proceedings
of the 2nd USENIX conference on Hot topics in cloud computing.
2010.</li>
<li>Karau, Holden. Fast Data Processing With Spark. Packt Publishing Ltd,
2013.</li>
<li>Gonzalez, Joseph E., et al. "Graphx: Graph processing in a distributed
dataflow framework." Proceedings of the 11th USENIX Symposium on
Operating Systems Design and Implementation (OSDI). 2014.</li>
<li>Zaharia, Matei, et al. "Resilient distributed datasets: A fault-tolerant
abstraction for in-memory cluster computing." Proceedings of the 9th
USENIX conference on Networked Systems Design and Implementation.
USENIX Association, 2012.</li>
<li>Spark official documentation page: https://spark.apache.org/documentation.html</li>
</ol>
<br/>



Figures:<br/>
Figure 1: <http://spark.apache.org><br/>
Figure 2: <http://spark.apache.org/sql><br/>
Figure 3: <https://spark.apache.org/docs/latest/graphx-programming-guide.html><br/>
Figure 4: <https://spark.apache.org/docs/latest/streaming-programming-guide.html><br/>
Figure 5: <https://spark.apache.org/docs/latest/streaming-programming-guide.html><br/>
Figure 6: <http://aws.amazon.com/articles/4926593393724923><br/>
