<!-- .slide: class="title"  -->
<!-- .slide: data-background-image="./images/spark-logo-rev.svg" data-background-size="100%" data-background-opacity="0.05" -->
<h2>
    <span class="title-accent">//</span>
    A bit of practise
</h2>



# Let's dive !
<!-- .slide: style="text-align: left" -->
* Launch a `spark-shell`

![Spark-shell Spark](./images/hands-on/prompt.png)

Find Spark-ui address and open it
<!-- .element: style="text-align: center" -->

Notes:
Important stuff created for you
* Spark-ui
* SparkContext and SparkSession
* type `:help`to discover, paste mode can be handy



# Run our first Job

Create a new dataset of integers
```scala
// Not mandatory but almost always necessary in scala
import spark.implicits._
// Create dataset of integers
val intsDF = (0 to 100000).toDF
//check schema
intsDF.printSchema
// groupBy odd/even number 
val result = intsDF.groupBy(($"value" % 2).as("even/odd")).agg(collect_list("value").as("ints"))
//check schema
result.printSchema
```


Notes:
* Alternative : val dsInts = spark.createDataframe(0 to 100000) 
and many others
* Explain `$` implicits and column renaming
* Ask to check Spark-ui : nothing Happens 



# Spark is Lazy

It will stack up all `transformations`, optimize them and then do the work

An `Action` is requried to trigger computation 

Notes:
* Explain transformations vs actions



# Trigger an action
```scala
result.show()
```
<img src="./images/hands-on/dag.png" with="20%" />

Notes:
* `show` is an action, as we ask spark to show the data, it must compute them
* The DAG is how spark compute data from the previous code



# Understanding your first app : 
<font size=7>`SparkSession`</font>
<!-- .slide: style="text-align: left" -->
Launching `spark-shell` means :
- Connect to a master and Create a `SparkSession`
- Starting an interactive application

Notes:
- Connecting to a master is alaways done
- We don't have a cluster so, we used a local master



# Understanding your first app : 
<font size=7>`Transformations`</font>
<!-- .slide: style="text-align: left" -->
Using Dataframe API and use `tranformations` means :
- Spark is building a *`DAG`* i.e. *`D`*irected *`A`*cyclic *`G`*raph

A DAG represents all the things to be done



# Understanding your first app : 
<font size=7>`Actions`</font>
<!-- .slide: style="text-align: left" -->
Using Dataframe API and use `actions` means :
- Tell Spark to trigger job execution
- Spark will split the *DAG* into tasks and affect each task to a `partition`


Notes:
- Introduce DataFrame as Storage and what is a partition


