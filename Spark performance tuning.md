source: Spark Big Data Cluster Computing in Production, Cap 3

# Partitioning

■ The partitions are fragments into which the data in an RDD is split.  
■ When the DAG scheduler transforms the job into stages, each partition will be processed into a single task, with each task requires a core to execute.  
■ The performance of Spark application is dependent on the parallelism level and this relates to the RDD’s number of partitions.

■ If the number of tasks spawned for your job is smaller than the number of CPUs available CPUs, you won’t benefit from your entire computing power and the data in one partition will be larger causing a spilling over to disk and I/O overhead.  
■ If setting too many partitions, this will spawn lots of small tasks that would need to be sent to worker nodes to be 
executed. This will pose an overhead on driver for scheduling tasks.

■ It is also possible to boost performance by using Partitioners.  
■ Spark natively uses two already-built partitioners: HashPartitioner and RangePartitioner.  
■ You can write your own custom Partitioner (pair RDD only) when you have a good working knowledge of your use case.


# Data Shuffling

■ Shuffling is a highly expensive operation on wide transformations, so the focus should be to decrease as much as possible the number of shuffles and the amount of data that gets shipped across the cluster.

■ This process is expensive because it can involve data sorting and repartitioning, serialization and deserialization while sending it over the network, and data compression to reduce the I/O bandwidth and disk I/O operations.

■ This operation happens in reduce phase of map/reduce transformations, and is not a trivial one as large chunks of data have to be sent from one node to another one.

■ A “trick” to increase the performance is pre-partitioning RDDs before wide transformations including join, reduceByKey, groupByKey, cogroup, and so on.  
■ Another way to avoid large amounts of data being scattered across the worker nodes is to choose the right operators at the right 
time:
1) groupByKey versus reduceByKey
2) repartition versus coalesce
3) reduceByKey versus aggregateByKey

