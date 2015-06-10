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
