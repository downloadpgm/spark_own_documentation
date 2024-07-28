
## Main Dataframe Operations

1) create dataframe

val df = spark.createDataframe()  
val df = spark.read.format().option().load()  
val df = spark.read.[csv(), json(), parquet(), jdbc(), orc()]  
val df = spark.read.table()

2) manipulate dataframe rows

df.groupBy().agg()  
df.groupBy().pivot().agg()  
df.[cube(), rollup()].agg()  
df.select().  
   where().  
   orderBy()  
df.join(df1, expression, joinType)

3) manipulate dataframe columns

df.withColumn()
df.withColumnRename()
df.drop()

4) save dataframe

df.write.format().save()
  .write.format().saveAsTable()
  
5) catalog

spark.sql()
spark.catalog().listTables()
spark.catalog().listColumns()

6) actions

df.show()
df.[head(), first()]
df.count()
df.describe().show()
   
-------------

## Main RDD Operations

1) create rdd

val rdd = sc.parallelize()
val rdd = sc.textFile() / sc.wholeTextFile()

2) manipulate rdd

rdd.map, flatMap
rdd.filter
rdd.aggregate, fold, reduce
rdd.group
rdd.sort
rdd.zip

3) manipulate pair rdd

rdd.mapValues, flatMapValues
rdd.cogroup, join, leftOuterJoin, rightOuterJoin, fullOuterJoin
rdd.combineByKey, aggregateByKey, foldByKey, reduceByKey
rdd.countByKey
rdd.groupByKey
rdd.sortByKey

4) manipulate per partition

rdd.mapPartitions, zipPartitions, foreachPartition
rdd.partitionBy
rdd.repartition, coalesce

5) save rdd

rdd.saveAsTexFile, saveAsSequenceFile, saveAsOjectFile

6) actions

rdd.collect
rdd.first, take(n), top(n), takeOrdered(n)
rdd.reduce
