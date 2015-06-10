---
layout: post
title: "Spark Hello World - Part 3"
description: "Spark intro part 2"
category: [bigdata]
tags: [big data, spark, hello world, recommendations]
---


First, you need to go to the [Spark Downloads page](http://spark.apache.org/downloads.html) and then select the particular Spark release you want to install and the package type (you could choose to build it from source for various Hadoop versions or select a version pre built for a specific Hadoop version)


Next, unzip the file and move it to a convenient folder like `(/home/username/Programming)` or anything you are comfortable with
Spark runs on both Windows and Unix perfectly well. The only requirement in either case is that you need to have Java installed and in your system path or set up the JAVA_HOME environment variable. I set up the `JAVA_HOME` environment variable on a Mac. For doing this (steps for a Mac machine), you would need to open the file `~/.bash_profile` (create it if it does not exist) and add an entry as follows:


`export JAVA_HOME='/path/to/your/java/home'`


Now, write to the file and close the terminal.


In case of a Linux machine (tested this on Ubuntu), you could add environment variables by editing the `/etc/environment` file and add the same export JAVA_HOME line as above. After this run the command `source /etc/environment` to update the environment variables. That's it. You can check if the environment variables have been correctly set by typing `echo $JAVA_HOME`


Also important to note that Spark runs on Java versions 6 and above, Python 2.6 and above. You can check Scala compatibility on the Spark website as it varies with the Spark version you choose to install.


Now, before we start writing a standalone Spark application, you could choose to look at the example applications that are already provided as part of the Spark installation. Steps on how to run the example applications are provided for [Scala](https://github.com/apache/spark/tree/master/examples/src/main/scala/org/apache/spark/examples), [Java](https://github.com/apache/spark/tree/master/examples/src/main/java/org/apache/spark/examples) and [Python](https://github.com/apache/spark/tree/master/examples/src/main/python). You can execute the following commands to run the example apps in Scala (from the directory where you unzipped Spark):


`./bin/run-example SparkPi`


[SparkPi](https://github.com/apache/spark/blob/master/examples/src/main/scala/org/apache/spark/examples/SparkPi.scala) is an interesting program that iteratively computes the value of Pi. A good explanation of what the program does is available at <http://mathforum.org/library/drmath/view/51909.html>. 

We need a specific directory structure in place for the application to work correctly. CD into your application folder (in my case it is `TestApp/`. This is the directory structure that you should have when you run a `find .` in your application folder:

`$ find .`

`.`

`./simple.sbt`

`./src`

`./src/main`

`./src/main/scala`

`./src/main/scala/TestApp.scala`

I will explain simple.sbt in the upcoming sections. Before that, let us look at the source code for TestApp.scala [here](https://github.com/gkrishnan/TestApp/blob/master/src/main/scala/TestApp.scala) . Consider that you have a log file that you want to inspect for lines that contain "error". The script TestApp.scala will help us achieve just that and list the count of the total number of lines in the log file that contain the word error. The code has detailed comments as to what each line is doing. If anything is not clear, feel free to drop me a note.

	/* TestApp.scala */
	import org.apache.spark.SparkContext
	import org.apache.spark.SparkContext._
	import org.apache.spark.SparkConf
	
	object TestApp {
	  def main(args: Array[String]) {
	    val logFile = "/Users/gnambiar/Programming/TestApp/logFile" 
	    //The above filepath should be a path pointing to a log file in your system
            
	    val conf = new SparkConf().setAppName("Test Application")  
	    //Set the application name here and create a new Spark Configuration object
	    
	    val sc = new SparkContext(conf) 
	    // Create a Spark Context utilizing the conf that you just created
	    
	    val logData = sc.textFile(logFile, 2).cache() 
	    //cache this, for faster execution
	    
	    val numErrors = logData.filter(line => line.contains("error")).count()
	    //Gets the number of lines that contain 'error'
	    
	    println("Lines with ERROR: %s".format(numErrors))
	    //Prints the above information
	    }
	}


Next, we move on to simple.sbt. First, what is sbt? It stand for the Scala Build Tool which lives at <http://www.scala-sbt.org/>. This is a tool that helps us to build the project, package it and create a jar file that we submit to Spark using the spark-submit command. 

SBT has detailed instructions on how to set it up on your machine at <http://www.scala-sbt.org/0.13/tutorial/Setup.html>. If you remember, we were using a specific directory structure. This was needed due to sbt, since if the source files are not in the correct locations or directories they would be ignored and your application would not run as expected. In this tutorial, we would be focusing on a basic sbt file - simple.sbt and we don't have too many dependencies that would arise while using MLLib or other Spark modules, as I plan to cover these advanced topics later.

Let's look at the [simple.sbt file](https://github.com/gkrishnan/TestApp/blob/master/simple.sbt). This simple Scala Build Tool file has only a few parameters:


    name := "Test Application"
    
    version := "1.0"
    
    scalaVersion := "2.10.4"
    
    libraryDependencies += "org.apache.spark" %% "spark-core" % "1.3.1"




*name*: Name of your application<br/>
*version*: Version of the application<br/>
*scalaVersion*: Version of Scala that your application is using<br/>
*libraryDependencies*: The libraries that you need to run your application. More relevant for complex examples that make use of Spark modules<br/>


Now, we need to package our application and create a jar file. Now cd into the root of your application. For instance, if your application was called TestApp, then cd into TestApp/ and then run the following command:

`$ sbt package`

Once you run this command you will see a lot of INFO logs as follows:

    [info] Set current project to Test Application (in build file:/Users/gnambiar/Programming/TestApp/)
    [info] Updating {file:/Users/gnambiar/Programming/TestApp/}testapp...
    [info] Resolving org.fusesource.jansi#jansi;1.4 ...
    [info] Done updating.
    [info] Compiling 1 Scala source to /Users/gnambiar/Programming/TestApp/target/scala-2.10/classes...
    [info] Packaging /Users/gnambiar/Programming/TestApp/target/scala-2.10/test-application_2.10-1.0.jar ...
    [info] Done packaging.
    [success] Total time: 33 s, completed Jun 10, 2015 11:29:40 AM
    
    
Finally, run spark-submit. Please note that in the below script SPARK-HOME should point to where you installed/unzipped Spark:


    $ SPARK-HOME/bin/spark-submit \
       --class "TestApp" \
       --master local[4] \
       target/scala-2.10/simple-project_2.10-1.0.jar
       
If everything went right, congrats on running your first Spark application! You should see this as the output:


`Lines with ERROR: 1`


