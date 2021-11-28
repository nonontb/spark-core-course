<!-- .slide: class="title"  -->
<!-- .slide: data-background-image="./images/spark-logo-rev.svg" data-background-size="100%" data-background-opacity="0.05" -->
<h2>
    <span class="title-accent">//</span>
    Spark core concepts
</h2>



# Dataframe ancestor

*`R`*esilient *`D`*istributed *`D`*ataset => RDD

A `Dataframe` is based on RDD and inherits its properties:

* Fault-tolerant
* Distributed
* Immutable
* Lazy

Notes:
* RDD: Paper from Matei Zaharia one of the cofounder of Spark
* Spark holds the first implementation of RDD



# RDD API

As `Dataframe` ancestor, RDD is also a storage and an API

RDD is still usefull when:
* No schema is required
* Low-level transformation is needed
* Need to tell Spark *How* to compute 


Notes:
* Low-level transformation: Binary



# RDD

TODO

Notes:
TODO



# dependencies : wide vs narrows


TODO

Notes:
TODO



# partition


TODO

Notes:
TODO



# DAG

TODO

Notes:
TODO




# Transformations vs actions

TODO

Notes:
TODO




# Lazyness

TODO - Catalyst 

Demo

Notes:
TODO







# shuffle

TODO

Notes:
TODO



# serialization

TODO

Notes:
TODO
