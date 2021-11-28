<!-- .slide: class="title"  -->
<!-- .slide: data-background-image="./images/spark-logo-rev.svg" data-background-size="100%" data-background-opacity="0.05" -->
<h2>
    <span class="title-accent">//</span>
    Why Spark ?
</h2>



# Data growth

Data growth is huge and hardware can't keep up

<font size=2>[Moore's law](https://fr.wikipedia.org/wiki/Loi_de_Moore) reach its limits</font> 


Notes:

- Moore's law  => Double number of transistors each two years
- law stops to be accurate around 2017
- Limits reach around now



# Big data 3V's
<!-- .slide: style="text-align: left" -->

* Volume => More and more data is produced and required
* Velocity =>  Need for data to be processed rapidly, instantly...
* Variety => text, binary (photos, videos), structured vs unstructured,... 


Notes:
Devices :
* Produce: Smart devices explosion, IoT,...
* Deep learning models training

Extended today as 5V's
* Veracity
* Value
This 2 last V's are not useful now, but can be introduced



# Hardware evolution


|        |2010          | 2015              | ~2020           |
|--------|:------------:|:-----------------:|:---------------:|
|Disk    | 50MBps (HDD) | 500 MBps/s (SSD)  | 3G GBps/s (SSD) |
|Network | 1Gbps        | 10 Gbps           | 100 Gbps        |
|CPU     | ~3Ghz        | ~3GHz / cores     | ~4Ghz /core ++  |



# Big data new paradigme

With such vaste amount of data, It becomes more interesting to move code instead of data


Notes:
- Beginning Hadoop (2006): How much time do we need to transfer 1 TB ? Today 1PB ?
- Spark as a cluster framework do the same = Introduce Serialization and code moving



# Hardware costs
<!-- .slide: style="text-align: left" -->
* A cluster of small computer are cheaper than big computer
* Memory prices dropped


Notes:

* Cheaper hardware as "pizza box" hadoop cluster 
* Memory price in 2020 * 2 = Memory price in 2005
* Memory price in 2005 * 5 = Memory price in 2000   



# Hadoop cluster limitations
<!-- .slide: style="text-align: left" -->
* Hadoop cluster are I/O bound
* Spark :
  * Ease of use
  * Powerful APIs
  * Faster
  * Much more features : Analytics, Spark-shell, ...


Notes:
* Faster: Spark use memory vs Hadoop use disk
* Ease of use
  * Many programming language : Java, Scala, Python, SQL, ..
* Pwerful APIs
  * RDD API : use as a non distributed collection API
  * SQL API & Mlib : not only for data engineer
* Much more features : Hadoop requires Hive, Beeline, Tez




# All put together


<h3>
  Need for a new generation cluster computing framework
</h3>

<img src="./images/spark-logo-rev.svg" class="centered" width=40% />
