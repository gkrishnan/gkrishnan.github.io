---
layout: post
title: "Spark Hello World"
description: "Spark intro part 2"
category: [bigdata]
tags: [big data, spark, hello world, recommendations]
---


First, you need to go to the [Spark Downloads page](http://spark.apache.org/downloads.html) and then select the particular Spark release you want to install and the package type (you could choose to build it from source for various Hadoop versions or select a version pre built for a specific Hadoop version)


Next, unzip the file and move it to a convenient folder like (/home/username/Programming) or anything you are comfortable with
Spark runs on both Windows and Unix perfectly well. The only requirement in either case is that you need to have Java installed and in your system path or set up the JAVA_HOME environment variable. I set up the JAVA_HOME environment variable. For doing this, you would need to open the file ~/.bash_profile (create it if it does not exist) and add an entry as follows:


`export JAVA_HOME='/path/to/your/java/home'`

Write to file and close the terminal. Re open the terminal and you are all set to run Spark now. Also important to note that Spark runs on Java versions 6 and above, Python 2.6 and above. You can check Scala compatibility on the Spark website as it varies with the Spark version you choose to install.

Now, getting on with the program,
