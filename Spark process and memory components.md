
(source: Spark Big Data Cluster in Production, page 24, 54)

Spark Driver

- maintain connection to all worker nodes
- creates an execution plan (DAG) based on logical code of application into physical commands
- handles communication with cluster manager to request resources for executors

  - Spark context:
    - maintains program execution state to assign tasks to executors 
    - maintains accumulators and broadcast variables
    - config settings of spark app and env
    - list of resources (cpu and memory) available in worker nodes
  
  - DAG scheduler:
    - breaks a DAG down into individual stages and tasks each time a action/job is triggered
    - determines the preferred locations to run tasks
    - handles failures in case shuffle output files being lost
  
  - Task scheduler:
    - is aware of resource and locality constraints 
	- decides where tasks should execute on which executors accordingly

	
Spark Executor:
- runs as a single JVM and has fixed set of resources (CPU cores and memory)
- saving intermediary results in memory by persisting/caching data


---------------

spark.executor.memory (default 512M)
  - spark.storage.memoryFraction (default 0.6)
    - spark.storage.safetyFraction (0.9)
  - spark.shuffle.memoryFraction (default 0.2)
    - spark.shuffle.safetyFraction (0.8)

spark.yarn.executor.memoryOverhead (10% max 384M) (additional if run YARN cluster)

spark.driver.memory (default 1G)
spark.yarn.driver.memoryOverhead (additional if run YARN cluster)
