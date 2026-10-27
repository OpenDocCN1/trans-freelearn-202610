
## Spark solutions

Spark is directly available from `spark.apache.org` as an open-source solution. **Databricks** is the leading provider of the commercial solution of Spark. For those who are familiar with programming in Python, R, Java, or Scala, the time required to start using Spark is minimal due to efficient interfaces, such as the PySpark API that allows users to work in Spark using just Python.

Cloud-based Spark platforms, such as the Databricks Community Edition, provide an easy and simple means to work on Spark without the additional work of installing and configuring Spark. Hence, users who wish to use Spark for programming and related tasks can get started much more rapidly without spending time on administrative tasks.

## Spark practicals

In this section, we will create an account on Databricks’ Community Edition and complete a hands-on exercise that will walk the reader through the basics of actions, transformations, and RDD concepts in general.

- 2. Click on the START TODAY button and enter your information:

![](img/8ededb1e0b69e85687f6e488d1044b79_219_0.png)

- 3. Confirm that you have read and agree to the terms in the popup menu (scroll down to the bottom for the **Agree** button):

### Terms of Service

Databricks Community Edition
Terms of Service

Welcome to Databricks Community Edition! We are pleased to provide Databricks Community Edition “Community Edition” at no charge to those interested in learning and exploring the use of Databricks’ cloud-based data analytics platform, which enables data analysts and others to easily tap the power of Apache Spark™ and Databricks’ other proprietary functionality. Your use of Community Edition, whether on an ongoing basis or on a temporary basis, for the purposes of training Databricks’ fee-based platform services, or your temporary, no-charge trial of our fee-based platform services apart from Community Edition (“Free Trial”) and, together with any use of Community Edition, the “Services” is governed by these Terms of Service (the “Terms”). By using the Services, you are agreeing to be bound by these Terms. If you are using the Services on behalf of an organization, you are agreeing to these Terms on behalf of that organization and you are confirming that you are authorized to bind to these Terms in which case “you” or “your” refer to that organization rather than you as an individual. Your access to the Services is contingent upon your reading these Terms carefully and, if you agree to them, clicking on the boxes at the bottom of each of the two sets of provisions below to indicate that you understand, accept and will continually abide by these Terms. If in the future Databricks agrees to sell any paid upgrade or other paid services to your Community Edition or Free Trial account, these Terms shall continue to apply in full unless you and Databricks mutually agree to be bound under a superseding set of governing terms and conditions.

### YOUR DATA AND USE OF COMMUNITY EDITION – RESTRICTIONS APPLY

- 4. Check your email for a confirmation email from Databricks and click on the link to confirm your account:

![](img/8ededb1e0b69e85687f6e488d1044b79_221_0.png)

- 5. Once you click on the link to confirm your account, you'll be taken to a login screen where you can log on using the email address and password you used to sign up for the account:

![](img/8ededb1e0b69e85687f6e488d1044b79_221_1.png)

- 6. After logging in, click on Cluster to set up a Spark cluster, as shown in the following figure:

![](img/8ededb1e0b69e85687f6e488d1044b79_222_0.png)

- 7. Enter `Pack_Exercise` as the Cluster Name and click on the Create Cluster button at the top of the page:

![](img/8ededb1e0b69e85687f6e488d1044b79_223_0.png)

- 8. This will initiate the process of starting up a Spark Cluster on which we will execute our Spark commands using an iPython notebook. An iPython Notebook is the name given to a commonly used IDE - a web-based development application used for writing and testing Python code. The notebook can also support other languages through the use of kernels, but for the purpose of this exercise, we will focus on the Python kernel.

After a while, the Status will change from Pending to Running:

![](img/8ededb1e0b69e85687f6e488d1044b79_224_0.png)

Status changes to Running after a few minutes:

![](img/8ededb1e0b69e85687f6e488d1044b79_224_1.png)

- 9. Click on Workspace (on the left hand bar) and select options, Users | (Your userid) and click on the drop-down arrow next to your email address. Select Create | Notebook:

![](img/8ededb1e0b69e85687f6e488d1044b79_225_0.png)

- 10. In the popup screen, enter Packt_Exercise as the name of the notebook and click on the Create button:

![](img/8ededb1e0b69e85687f6e488d1044b79_225_1.png)

- 11. Once you click on the **Create** button, you'll be taken directly to the Notebook as shown in the following screenshot. This is the Spark **Notebook**, where you'll be able to execute the rest of the code given in the next few sections. The code should be typed in the **cells** of the notebook as shown. After entering your code, press *Shift + Enter* to execute the corresponding cell:

![](img/8ededb1e0b69e85687f6e488d1044b79_226_0.png)

- 12. For the next few exercises, you can copy-paste the text into the cells of the Notebook. Alternatively, you can also import the notebook and load it directly in your workspace. If you do so, you'll not need to type in the commands (although typing in the commands will provide more hands-on familiarity).

- 13. An alternative approach to copy-pasting commands: You can import the notebook by clicking on Import as shown in the following screenshot:

![](img/8ededb1e0b69e85687f6e488d1044b79_226_1.png)

Documentation for Spark's Python API can be found at https://spark.apache.org/docs/latest/api/python/pyspark.html#pyspark.sql.

The data for this book was imported into the Databricks' Spark Platform. For more information on importing data, go to Importing Data - Databricks (https://docs.databricks.com/user-guide/importing-data.html).

```python
# COMMAND ----------
# The SparkContext/SparkSession is the entry point for all Spark operations
# sc = the SparkContext = the execution environment of Spark, only 1 per JVM
# Note that SparkSession is now the entry point (from Spark v2.0)
# This tutorial uses SparkContext (was used prior to Spark 2.0)

from pyspark import SparkContext
# sc = SparkContext(appName = "some_application_name") # You'd normally run this, but in this case, it has already been created in the Databricks' environment

# COMMAND ----------

quote = "To be, or not to be, that is the question: Whether 'tis nobler in the mind to suffer The slings and arrows of outrageous fortune, Or to take Arms against a Sea of troubles, And by opposing end them: to die, to sleep No more; and by a sleep, to say we end the heart-ache, and the thousand natural shocks that Flesh is heir to? 'Tis a consummation devoutly to be wished. To die, to sleep, To sleep, perchance to Dream; aye, there's the rub, for in that sleep of death, what dreams may come, when we have shuffled off this mortal coil, must give us pause."

# COMMAND ----------
sparkdata = sc.parallelize(quote.split(' '))

# COMMAND ----------
print "sparkdata = ", sparkdata
print "sparkdata.collect = ", sparkdata.collect
print "sparkdata.collect() = ", sparkdata.collect()[1:10]

# COMMAND ----------
# A simple transformation - map
def mapword(word):
    return (word,1)

print sparkdata.map(mapword) # Nothing has happened here
print sparkdata.map(mapword).collect()[1:10] # collect causes the DAG to execute

# COMMAND ----------
# Another Transformation

def charsmorethan2(tuple1):
    if len(tuple1[0])>2:
        return tuple1
    pass

rdd3 = sparkdata.map(mapword).filter(lambda x: charsmorethan2(x))
# Multiple Transformations in 1 statement, nothing is happening yet
rdd3.collect()[1:10]
# The DAG gets executed. Note that since we didn't remove punctuation marks
# ... 'be,', etc are also included

# COMMAND ----------
# With Tables, a general example
cms = sc.parallelize([[1,"Dr. A",12.50,"Yale"],[2,"Dr. B",5.10,"Duke"],[3,"Dr. C",200.34,"Mt. Sinai"],[4,"Dr. D",5.67,"Duke"],[1,"Dr. E",52.50,"Yale"]])

# COMMAND ----------
def findPayment(data):
    return data[2]

print "Payments = ", cms.map(findPayment).collect()
print "Mean = ", cms.map(findPayment).mean() # Mean is an action

# COMMAND ----------
# Creating a DataFrame (familiar to Python programmers)

cms_df = sqlContext.createDataFrame(cms, ["ID","Name","Payment","Hosp"])
print cms_df.show()
print cms_df.groupby('Hosp').agg(func.avg('Payment'), func.max('Payment'),func.min('Payment'))
print cms_df.groupby('Hosp').agg(func.avg('Payment'), func.max('Payment'),func.min('Payment')).collect()
print
print "Converting to a Pandas DataFrame"
print "-------------------------------"
pd_df = cms_df.groupby('Hosp').agg(func.avg('Payment'),func.max('Payment'),func.min('Payment')).toPandas()
print type(pd_df)
print
print pd_df

# COMMAND ----------
wordsList = ['to','be','or','not','to','be']
wordsRDD = sc.parallelize(wordsList, 3) # Splits into 3 groups
```
