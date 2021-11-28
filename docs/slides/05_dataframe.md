<!-- .slide: class="title"  -->
<!-- .slide: data-background-image="./images/spark-logo-rev.svg" data-background-size="100%" data-background-opacity="0.05" -->
<h2>
    <span class="title-accent">//</span>
    Dataframe discovery
</h2>



# Dataframe representation

<img src="./images/dataframe/dataframe.drawio.svg"  width=60% />

Notes:
When we speak about dataframe, we can reference it as :
- An API
- A data structure
- A distributed storage



# Dataframe API

or `DataSet<Row>`

As a Spark developer, you will mainly use the API to manipulate
columns and rows

`Dataframe` API is used in all Spark components

Notes:
* in Scala Dataframe is just an alias
* Spark components: Spark SQl, Streaming, MLIb, Graphx



# Table representations

* Data is described as rows made of columns
* Each Columns are strongly Typed
* Each Column must match the provided Schema, if not provided a schema is inferred


Notes:
- Apache Data Types => Conversion from primitive
- A Row is a built from `Seq[Any]` and can be associated with a Schema



# Distributed storage

A `Dataframe` is divided into partitions located on different cluster nodes

Each partition holds a subset of the distributed Dataset and may be stored:
* In memory
* On Disk
* Both

Notes:
* Partition can also be serialized and compressed



<!-- .slide: data-auto-animate id="immutability" -->
# Immutability
`Dataframe` are immutable => It cannot be changed

The question is How to modify an immutable `Dataframe`?

<div style="opacity: 0;">Spark stores only the *transformations* but not the *transformed data*</div>

Notes:
* Transformations like a pipeline blue print




<!-- .slide: data-auto-animate -->
# Immutability
`Dataframe` are immutable => It cannot be changed

The question is How to modify an immutable `Dataframe`?

Spark stores only the *transformations recipe* but not the *transformed data*


Notes:
* Sharing the blue print means sharing the code, it means code need to be
serializable
* Spark uses immutability as one of its pillar to optimize data processing through *Catalyst engine*. As a developer, you don't have to worry about it too much
* More on next chapter



# Dataframe API: Ingestion

From a lot different sources as files, databases, flow and even custom datasources

It always use the same pattern:
<!-- .element: style="text-align: left;" -->
File based:
<!-- .element: style="text-align: left;" -->
```scala
val df = spark.read // Get a Dataframe reader
  .options(<map_options>)  // configure the datasource
  .format(<supported_datasource>) // tell spark which datasource to use
  .load(<paths>) // provides path
```

Jdbc based:
<!-- .element: style="text-align: left;" -->
```scala
val df = spark.read // Get a Dataframe reader
  .jdbc(<url>, <table>, <connection_properties>) // required jdbc properties
```


Notes:
- Spark provide a Datasource API to use dataframe API on not supported sources
- `.format()` and `.load()`can be shorten by an alias for supported file-based datasource. ex: `.parquet(<path>)`



# Dataframe API: Storing

As a lot different as files, databases, flow and even custom datasources

It always uses the same pattern:
<!-- .element: style="text-align: left;" -->
File based:
<!-- .element: style="text-align: left;" -->
```scala
val df = spark.writer // Get a Dataframe writer
  .options(<map_options>)  // configure the datasource
  .mode(SaveMode.Append) // Specify how to write data
  .format(<supported_datasource>) // tell spark which datasource to use
  .save(<path>) // provides path
```

Jdbc based:
<!-- .element: style="text-align: left;" -->
```scala
val df = spark.read // Get a Dataframe reader
  .mode(SaveMode.Overwrite) // Specify how to write data
  .jdbc(<url>, <table>, <connection_properties>) // required jdbc properties
```


Notes:
- Spark provide a Datasource API to use dataframe API on not supported sources
- `.format()` and `.save()`can be shorten by an alias for supported file-based datasource. ex: `.parquet(<path>)`
- SaveMode:
  - Overwrite
  - Append
  - ErrorIfExists
  - Ignore

# Dataframe API: Storing
<!-- .element: style="text-align: left;" -->
Supported most common datasources
- Built-in:
  - Parquet
  - ORC
  - Json
  - CSV
  - Plain text
  - Hive
  - JDBC
- Module:
  - Avro (external module)



# Dataframe API: Schema
<!-- .slide: style="font-size: 0.8em;" -->
`Dataframe` schema can be viewed using:
<!-- .element: style="text-align: left;" -->
```scala
df.printSchema()
```
Schema is displayed like a tree as fields can be nested:
<!-- .element: style="text-align: left;" -->
```text
root
  |-- field1: string (nullable = false)
  |-- field2: integer (nullable = true)
  |-- parent: struct (nullable = false)
  |    |-- nested: float (nullabe = true)
```

It gives for each field:
<!-- .element: style="text-align: left;" -->
| Name | Type | Nullability |
|------|------|----------|
| field1 | string | false |
| field2 | string | true |
| parent | struct | false |
| nested | float | true |



# Dataframe API: Schema

At the API level, a schema is a  `StructType` containing a list of `StructField`
Previous example can be written:
<!-- .element: style="text-align: left;" -->
```scala
val schema = StructType(
    Seq(
        StructField("field1" ,StringType, nullable = false),
        StructField("field2", StringType),
        StructField("parent",
            StructType(
                Seq(
                    StructField("field1", StringType)
                )
            ),nullable = false)

    )
)
```

Notes:
-  nullable true is the default behavior



# Dataframe API: Transformations
<!-- .slide: style="text-align: left;font-size: 0.8em" -->
Some Methods and functions to manipulate dataframe.

Methods
- `withColumn()` : Create a new column from an *Expression* or a *Column*
- `withColumnRenamed()` : Rename a column
- `col()` : Gets a column by name
- `drop()` : Delete a column by name or *Column*

Functions:
- `lit()` : create a column from a literal value
- `when(<col>,<value>)`: Implements predicate on *Column* and create a new *Column* from a value.
- `otherwise(<col>)`: Implements *else* and create a new *Column*


Notes:
- All built-in functions are locates in `org.apache.spark.sql.functions._` package
- `col()`can be replaced by `$` when using `import spark.implicits._` in Scala
- `when` col predicate is false => default value null



# TP Dataframe: Schema

Clone the following repository
```bash
git@github.com:nonontb/spark-core-course-practise.git
```

Complete missing implementation marked as `???`
so that all tests passed.

Run the test: `sbt 'project dataframe' clean test`


Notes:
- A configured IDE with Sbt can run the test



# Dataframe API: Column

`DataFrame` API may use a *Column* or a *Column* name

A *Column* represent a field in the Schema

Spark provides many built-in functions which use *Column* to apply `DataFrame`transformations

Check [SQL Functions API](https://spark.apache.org/docs/latest/api/sql/index.html) or [Spark sources](https://github.com/apache/spark/blob/master/sql/core/src/main/scala/org/apache/spark/sql/functions.scala)



# Dataframe API: Column

Nested fields are accessed using `.` (dot) notation.

```scala
df.withColumn("myCol",df.col("nested.innerCol"))
```

But you can't create a *Column* with `.` notation. You have to create each `struct` parent col and field inside

Since Spark 3.1.0, convenience method were added :
- Create: `withField(<name>,<col>)`
- Delete: `dropFields(<names>*)`
- Fetch: `getField(<name>)`



# TP  Dataframe: Column

In previous cloned repository

Run the test: `sbt 'project columns' clean test`

Complete missing implementation marked as `???`
so that all tests passed.



# DataFrame vs Dataset

A `DataFrame` is a `DataSet<Row>`

`DataSet` API uses generic with a lot of reference to `DataSet<T>` where `T` is not just Row

`DataSet<T>` allows to reuse POJO from existing librairies representing a Business domain and more strongly typed

`DataFrame` API is more flexible as any transformation on a `Row` is still a `Row`

Notes:
- In scala `DataFrame`is just an Alias, we only call `DataFrame` a `DataSet<Row>`
- POJO or case class in Scala
- Better compile time check in scala
- Join or aggregation on a `DataSet<T>` will produce a `Row`



# DataFrame vs Dataset
<!-- .slide: style="text-align: left;" -->
Using `imports sparkSession.implicits._`
You can switch between `DataFrame` and `DataSet<Row>` easily:

- From `DataSet<T>` to `DataFrame`:

```scala
val ds = sparkSession.createDataset(...)
ds.toDF()
```

- From `DataFrame` to `DataSet<T>`:

```scala
val df = sparkSession.createDataFrame(Seq(Row(1)),schema)
val ds:DataSet[T] = df.as[T]
// Type definition must match schema
```

`imports sparkSession.implicits._` enables magic with encoders.

`T` must be a `case class` for conversion to work natively

Notes:
- In Java, Encoders must be provided
- Encoders: Introduce Tungsten but more in advanced concepts



# RDD vs DataSet/DataFrame
<!-- .slide: style="text-align: left;" -->
`RDD` is a Spark core concept

`RDD` is a low level API and mimic scala Collection API

 As a low level API`RDD` describes *How to do ...* as opposed to *What to do ... * from `DataFrame`

`RDD` is also a date structure and `DataSet/DataFrame`are based on it



# RDD vs DataSet/DataFrame
<!-- .slide: style="text-align: left;" -->
Again using `imports sparkSession.implicits._`
You can switch between `RDD` and `DataSet` easily:

- From `RDD<T>` to `DataSet<T>`:

```scala
val rdd:RDD[T] = sparkSession.sparkContext.parallelize(Seq(...))
val ds = sparkSession.createDataset(rdd)
```

- From `DataSet<T>` to `RDD<T>`:

```scala
val ds:DataSet[T] = sparkSession.createDataset(Seq(...))
val rdd:RDD[T] = ds.rdd
// Type definition must match schema
```

Notes:
- More on RDD in next section
- RDD: Resilient Distributed DataSet
- This is not a free transformation as data are not stored the same way (Java serialization vs Encoders)



# Aggregation

Aggregation is a very common part of data transformation and Spark offer many ways to do it

It is quite different using the different `RDD/DataFrame/DataSet` APIs



# Aggregation: RDD
<!-- .slide: style="text-align: left;" -->
- From a `RDD[V]`:

```scala
val rdd:RDD[V] = ???
// groupBy use a f:T -> K , K is type of the resulting Key
val aggRdd:RDD[(K,Iterable[V])] = rdd.groupBy(v:V => t....) 
// f:Iterable[V] => T, function   aggregation
val result:RDD[(K,V)] = aggRdd.mapValues(f:...) 

```

- From a `RDD[(K,V)]`:

```scala
val rdd:RDD[(K,V)] = ???
val aggRdd:RDD[(K,Iterable[V])] = rdd.groupByKey()
// f:Iterable[V] => T
val result:RDD[(K,T)] = aggRdd.mapValues(f...) 
```
or 
```scala
val rdd:RDD[(K,V)] = ???
val reduceRdd:RDD[(K,V)] = rdd.reduceByKey(f:(V,V) => V) // f:Iterable[V] => T, compute all grouped values to wanted result : count, min, max, ...
```

Notes:
- groupByKey vs reduceByKey optimization



# Aggregation: DataFrame
<!-- .slide: style="text-align: left;" -->
DataFrame uses *Column* describes in its ad-hoc schema

```scala
val dfGrouped = df.groupBy($"<col-name>")
val result = dfGrouped.agg(...) // 
```

`agg(...)` is flexible - parameters:
- A collection of String tuple (column,aggregate) or Map[String,String]
```scala
df.groupBy("group-col").agg(Map(
  "max-col" -> "max",
  "min-col" -> "min"
))
```
- A variadic `Columns` parameter ie 
```scala
df.agg(max(col("colname").as("max-col"),min(col("colname").as("min-col"))
```



# Aggregation: DataSet
<!-- .slide: style="text-align: left;" -->
`DataSet` can use a typed API as RDD or `DataFrame` API based on *Column*
- Like DataFrame :
```scala
val resultDS:Dataset[String] = dsAuthor.groupByKey(f:).agg(...)
val resultDS:Dataset[String] = dsAuthor.groupBy($"colname").agg(...)
```
- Like RDD:
```scala
// create a func f:(K,Iterator[V] => U), U requires an encoder
val resultDS:Dataset[String] = dsAuthor.groupByKey(f:).mapGroups(f:...)
// create a func f:(V,V => V)
val resultDS:Dataset[String] = dsAuthor.groupBy($"colname").reduceGroup(f:...)
```

agg(...) method takes `TypedColumn` as parameters

Notes:
- DataFrame can also use mapGroup or reduceGroup as DataSet[Row]
- Other flatMapGroups func



# Aggregation: Window Spec
<!-- .slide: style="text-align: left;" -->
`WindowSpec` is a specification that defines rows included in the Window

A `WindowSpec` can be defined using:
- Partition specification using a `Column` or an `Expr`
- Order specification
- Range/Row specification

Then you can apply an aggregate function using `Column.over` on the specified Window.

```scala
 val windowSpec:WindowSpec = Window.partitionBy("col-name").orderBy("col-name")
val df:Dataframe = df.withColumn("new-agg-col",aggfunc($"col").over(WindowsSpec))
```



# TP DataFrame: Aggregation

In previous cloned repository

Run the test: `sbt 'project aggregation' clean test`

Complete missing implementation marked as `???`
so that all tests passed.



# Join: Dataframe

Joining `Dataframe` means merging all fields from each `Dataframe` into one

all fields from each `Dataframe Row` will be merged into one `Row`

```scala
val left:Dataframe = ???
val right:Dataframe = ???
// joinExpr: column name, Expr, Col,... 
// jointype:inner, outer, left, ...
val joinDF = left.join(right,<joinExpr>,<joinType>)
```
Notes:
- If using same column on left/right => only one in result




# Join: Dataset

Joining `Dataset` means creating a tuple holding each `Dataset` inner type

The join result is a `Dataset` of tuple containing each `Dataset` type. 

At the `Schema` level, the result is a new `Row` containing two sub `Rows` named by default `_1` and `_2`

```scala
val left:Dataset[T] = ???
val right:Dataset[U] = ???
// joinExpr: column name, Expr, Col,... 
// jointype:inner, outer, left, ...
val joinDS:Dataset[(T,U)] = left.joinWith(right,<joinExpr>,<joinType>)
```

Notes:
- DataSet can use join() as Dataframe=DataSet[Row]



# Join: Filter

Joining can filter one dataset from another using `anti` join type. 

All `Row` from the left dataset that do not find a matching `Row` in the right dataset using the joinExpr 
is filtered out

The resulting `Dataframe\Dataset` is the Schema/Type as the left `Dataframe\Dataset`

```scala
val left:Dataframe = ???
val right:Dataframe = ???
// joinExpr: column name, Expr, Col,... 
// jointype:inner, outer, left, ...
val joinDF = left.join(right,<joinExpr>,"anti")

# TP DataFrame: Join

In previous cloned repository

Run the test: `sbt 'project join' clean test`

Complete missing implementation marked as `???`
so that all tests passed.