
Cluster Manager

■ Purpose is to allocate physical resources (CPU and memory) that can execute logical code.

■ It has to deal with following requirements: 
1) scalability (adding hardware resources, especially in a network, we introduce complexity to the system)
2) durability (being resilient to failures both on any single machine, and on failures between machines, or with degraded performance)
3) availability (system remains operational even failures of individual components without interruption of service, with dynamic addition and loss of resources)

■ Cluster manager ensures that multiple applications running in a cluster can, at best, run simultaneously, or, at worst, that all applications will have the resources they need to complete (memory, CPU, disk, and network resources).

■ Cluster manager ensures that resources in a distributed environment are available when many concurrent users share this environment. When many users share the same environment, they can make conflicting requests (such as requesting 70% of the available 
memory of the system at the same time). Cluster manager can deal with this conflict by explicitly scheduling when the users get specific resources, or by negotiating with the requesting user or application.

Driver

■ Driver is essentially an overseer that maintains connections to the other entities in the cluster and submits tasks for execution on the worker nodes in the cluster.
■ Translates the logical Spark application code into physical commands that are executed in the cluster.
■ Maintains the Spark context — a program state that tracks application settings and available resources, and maintains certain internal constructs, such as accumulators and broadcast variables.
■ Handles communication with the cluster manager to request resources to execute the Spark application.
■ Creates an execution plan based on the code of Spark application and submits to worker nodes. The execution plan is a DAG of actions and transformations. Spark optimizes this DAG to minimize data movement.
■ An internal component known as the DAG Scheduler breaks this DAG down further into individual stages and then tasks.
■ Then, the Task Scheduler handles the scheduling of these tasks in the cluster taking into account resource and locality constraints and assigns tasks to executors accordingly.


Workers and Executors

■ worker nodes are the physical machines that run executors, which have one or more tasks, executing the code that makes up a Spark job.
■ Each worker has a fixed set of resources available to it (CPU and memory), and these resources are explicitly allocated to the cluster manager.
■ Each worker node in a Spark cluster may run one more Spark executors — executors are an abstraction that runs in a single Java Virtual Machine (JVM) and has a fixed set of resources.
■ Since the data being processed in a cluster is split up across multiple nodes, depending on the data size and how it is distributed, you may achieve better performance by increasing the number of executors.
■ A single executor may have multiple tasks sharing a memory pool. This memory pool can be used to cache data, shuffle operations and shared variables along the cluster.