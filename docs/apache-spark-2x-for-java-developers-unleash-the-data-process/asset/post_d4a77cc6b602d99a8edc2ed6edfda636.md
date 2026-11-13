
```
# Alias definitions.
# You may want to put all your additions into a separate file like
# ~/.bash_aliases, instead of adding them here directly.
# See /usr/share/doc/bash-doc/examples in the bash-doc package.

if [ -f ~/.bash_aliases ]; then
    . ~/.bash_aliases
fi

# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

export SCALA_HOME=/usr/local/scala-2.11.8
export PATH=$PATH:/usr/local/scala-2.11.8/bin
```

Now, we have set up a Scala environment successfully. So, it is time to download Apache Spark. You can download it from http://spark.apache.org/downloads.html.

![](img/a77bd9f099223ea139d6d612f2b90712_123_0.png)

*The Spark version can be different, as per requirements.*

After Apache Spark is downloaded, run the following commands to set it up:

```
tar -zxf spark-2.0.0-bin-hadoop2.7.tgz
sudo mv spark-2.0.0-bin-hadoop2.7 /usr/local/spark
```

![](img/a77bd9f099223ea139d6d612f2b90712_123_1.png)

*Directory location can be different as per user's requirement.*

Also, you can set environment variable SPARK_HOME. It is not mandatory; however, it helps the user to find the installation directory of Spark. Also, you can add the path of Spark binaries in the $PATH variable for accessing them without specifying their path:

```
export SPARK_HOME=/usr/local/spark
export PATH=$PATH:/usr/local/scala-2.11.8/bin:$SPARK_HOME/bin
```

It is shown in the following screenshot:

```
# enable programmable completion features (you don't need to enable
# this, if it's already enabled in /etc/bash.bashrc and /etc/profile
# sources /etc/bash.bashrc).
if ! shopt -oq posix; then
  if [ -f /usr/share/bash-completion/bash_completion ]; then
    . /usr/share/bash-completion/bash_completion
  elif [ -f /etc/bash_completion ]; then
    . /etc/bash_completion
  fi
fi

export SCALA_HOME=/usr/local/scala-2.11.8
export SPARK_HOME=/usr/local/spark
export PATH=$PATH:/usr/local/scala-2.11.8/bin:$SPARK_HOME/bin
```

Now, we are ready to start Spark in standalone mode. Let's run the following command to start it:

```
$SPARK_HOME/bin/spark-shell
```

Also, we can simply execute the spark-shell command as Spark binaries are added to the environment variable PATH.

```
sparkuser@spark$
sparkuser@spark$
sparkuser@spark$
sparkuser@spark$
sparkuser@spark$ spark-shell
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel). For SparkR, use setLogLevel(newLevel).
17/05/22 06:49:41 WARN NativeCodeLoader: Unable to load native-hadoop library for your platform... using builtin-java classes where applicable
17/05/22 06:49:45 WARN Utils: Your hostname, desa1001 resolves to a loopback address: 127.0.1.1; using 10.204.136.224 for deduplication instead
Spark context Web UI available at http://10.204.136.224:4040
Spark context available as 'sc' (master = local[*], app id = local-1495435781992).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /__ / .__/\_,_/_/ /_/_\_\   version 2.1.1
      /_/


Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_121)
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

Also, you can access Spark Driver's UI at http://localhost:4040:

![](img/a77bd9f099223ea139d6d612f2b90712_125_0.png)

We will discuss more about Spark UI in the *Spark Driver Web UI* section of this chapter.

In this section, we have completed the Spark setup in standalone mode. In the next section, we will do some hands on Apache Spark, using `spark-shell` or `spark-cli`.

## Spark REPL also known as CLI

In Chapter 1, Introduction to Spark, we learnt that one of the advantages of Apache Spark over the MapReduce framework is interactive processing. Apache Spark achieves the same using Spark REPL.

Spark REPL or Spark shell, also known as Spark CLI, is a very useful tool for exploring the Spark programming. REPL is an acronym for Read-Evaluate-Print Loop. It is an interactive shell used by programmers to interact with a framework. Apache Spark also comes with REPL that beginners can use to understand the Spark programming model.

To launch the Spark REPL, we will execute the command that we executed in the previous section:

```
$SPARK_HOME/bin/spark-shell
Using Spark's default log4j profile: org/apache/spark/log4j-defaults.properties
Setting default log level to "WARN".
To adjust logging level use sc.setLogLevel(newLevel).
16/11/01 16:38:43 WARN NativeCodeLoader: Unable to load native-hadoop library for y
16/11/01 16:38:43 WARN Utils: Your hostname, dlla1424 resolves to a loopback addres
16/11/01 16:38:43 WARN Utils: Set SPARK_LOCAL_IP if you need to bind to another add
16/11/01 16:38:45 WARN SparkContext: Use an existing SparkContext, some configurati
Spark context Web UI available at http://192.168.0.15:4040
Spark context available as 'sc' (master = local[*], app id = local-1477998525363).
Spark session available as 'spark'.
Welcome to
      ____              __
     / __/__  ___ _____/ /__
    _\ \/ _ \/ _ `/ __/  '_/
   /___ / .__/\_,_/_/ /_/\_\   version 2.1.1.
      /_/

Using Scala version 2.11.8 (Java HotSpot(TM) 64-Bit Server VM, Java 1.8.0_101)
Type in expressions to have them evaluated.
Type :help for more information.

scala>
```

## Some basic exercises using Spark shell

Note that Spark shell is available only in the Scala language. However, we have kept examples easy to understand by Java developers.

## Checking Spark version

Execute the following command to check the Spark version using spark-shell:

```
scala>sc.version
res0: String = 2.1.1
```

It is shown in the following screenshot:

## Creating and filtering RDD

Let's start by creating an RDD of strings:

```scala
scala>val stringRdd=sc.parallelize(Array("Java", "Scala", "Python", "Ruby","JavaScript
stringRdd: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[0] at paralleli
```

Now, we will filter this RDD to keep only those strings that start with the letter `J`:

```scala
scala>val filteredRdd = stringRdd.filter(s => s.startsWith("J"))
filteredRdd: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[2] at filter at <c
```

In the first chapter, we learnt that if an operation on RDD returns an RDD then it is a transformation, or else it is an action.

The output of the preceding command clearly shows that filter the operation returned an RDD so the filter is a transformation.

Now, we will run an action on `filteredRdd` to see it's elements. Let's run `collect` on the `filteredRdd`:

```scala
scala>val list = filteredRdd.collect
list: Array[String] = Array(Java, JavaScript, Java)
```

As per the output of the previous command, the `collect` operation returned an array of strings. So, it is an action.

Now, let's see the elements of the `list` variable:

```scala
scala> list
res5: Array[String] = Array(Java, JavaScript, Java)
```

We are left with only elements that start with `J`, which was our desired outcome:

```
scala> val stringRdd=sc.parallelize(Array("Java","Scala","Python","Ruby","JavaScript","Java"))
stringRdd: org.apache.spark.rdd.RDD[String] = ParallelCollectionRDD[7] at parallelize at <console>:24

scala> val filteredRdd = stringRdd.filter(s => s.startsWith("J"))
filteredRdd: org.apache.spark.rdd.RDD[String] = MapPartitionsRDD[8] at filter at <console>:26

scala> val list = filteredRdd.collect
list: Array[String] = Array(Java, JavaScript, Java)

scala> list
res3: Array[String] = Array(Java, JavaScript, Java)

scala> 
```
