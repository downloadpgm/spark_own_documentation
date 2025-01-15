Source: Spark Big Data Cluster Computing in Production, Cap 3

## Partitioning

■ The partitions are fragments into which the data in an RDD is split.  
■ When the DAG scheduler transforms the job into stages, each partition will be processed into a single task, with each task requires a CPU core to execute.  
■ The performance of Spark application is dependent on the parallelism level and this relates to the RDD’s number of partitions.

■ If the number of tasks spawned for your job is smaller than the number of CPUs available, you won’t benefit from your entire computing power, and the data in one partition will be larger causing a spilling over to disk and I/O overhead.  
■ On the other hand, setting too many partitions will spawn lots of small tasks that would need to be sent to worker nodes to be executed. This will pose an overhead on driver for scheduling tasks.

■ It is also possible to boost performance by using Partitioners.  
■ Spark natively uses two already-built partitioners: HashPartitioner and RangePartitioner.  
■ You can write your own custom Partitioner (pair RDD only) when you have a good working knowledge of your use case.


## Data Shuffling

■ Shuffling is a highly expensive operation on wide transformations, so the focus should be to decrease as much as possible the number of shuffles and the amount of data that gets shipped across the cluster.

■ This process is expensive because it can involve data sorting and repartitioning, serialization and deserialization while sending it over the network, and data compression to reduce the I/O bandwidth and disk I/O operations.

■ This operation happens in reduce phase of map/reduce transformations, and is not a trivial as large chunks of data have to be sent from one node to another one.

■ A "trick" to avoid shuffling along wide transformations is pre-partitioning RDDs in operations including join, reduceByKey, groupByKey, cogroup, and so on.  
■ Another way to avoid large amounts of data being scattered across the worker nodes is to choose the right operators at right 
time:
1) groupByKey versus reduceByKey
2) repartition versus coalesce
3) reduceByKey versus aggregateByKey

■ Shuffling is not all bad. In the first place, shuffling is a necessary operation because it is the way Spark reorganizes the data on your cluster. Second, increasing the parallelism level could be a real performance booster by repartition the RDDs and shuffled across the cluster.


## Serialization

■ Spark is a distributed system and transfers data across the cluster over the network, cache it in memory, or spill it to disk.

■ Spark has two forms for data records: serialized binary representation, and deserialized Java object representation. Spark uses 
the deserialized representation when it handles the data in memory and only serializes it when shipping it between nodes across a network, or when it writes it to disk.

■ By default, Java serializer is used, but this option is slow and the objects of many classes may end up being serialized in large formats.

■ Kryo serializer significantly improves the speed of the Java serializer and outputs a more compact binary representation of the objects. However, you have to register the custom classes in advance to this serializer to have this benefit.


## Caching

■ Spark supports caching intermediary results in memory. RDD partitions will be stored in memory or on disk on the node that computed them.

■ For future actions on top of persisted dataset or on others derived from it, Spark won’t re-compute it and return the data from the persisted partitions.

■ To make an RDD persisted, you call the cache() or persist() methods on it. 
By default, they both store the computed RDD in memory but persist() allows persistence in memory, disk, or mixed of both.


## Memory Management

■ Spark makes use of large-scale memory on the cluster to efficiently leverage performance on processing data.

■ By default, Spark will give 60 percent from the Java heap to persistence, 20 percent to shuffles, and its remaining 20 percent for 
executing the user code.

■ Gargage collection highly impacts Spark performance as the application threads are stopped while the objects in the GC Old generation are organized.

■ To avoid performance loss, make sure that you are efficiently using the heap memory for caching.  
■ If you leave more heap space for the program’s execution, garbage collection will be more efficient. But if you allocate too much heap space for caching, then you might end up with a large number of objects in the GC old generation, leading to a great performance loss


## Data Locality

■ Spark takes into account data locality when scheduling the task execution based on the distribution of the dataset across the cluster and available resources.

■ This is also how Spark operates: It moves the serialized code to the workers where the data is placed.

■ Spark schedules the task execution so that it obtains the closest locality. However, it has to give up on a close locality in favor of a farther one because of resources reasons.  
■ If the executor doesn’t become available on machine where the data that has to be processed is busy, then a new task will be launched on one of the available nodes.