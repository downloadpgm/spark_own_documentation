
A Spark cluster is a set of interconnected JVM processes, usually running in a distributed manner on different machines.

The main clusters that Spark runs on are Hadoop YARN, Apache Mesos, and Spark standalone.


# Spark Standalone

Source: Spark in Action, cap 11

■ Standalone cluster is built specifically for Spark applications and only run Spark applications.  
■ A Spark standalone cluster provides faster job startup than those jobs running on YARN.  
■ It’s relatively simple and efficient to setup and comes with Spark out of the box.  
■ Spark has to be installed on all nodes in the cluster in order for them to be usable as slaves.

Limitations:
1) It is not possible to run mixed different types of Java applications, only Spark.
2) It can not run intermixed different versions of either Spark applications or Standalone clusters.
3) To allow concurrent run of Spark applications in the cluster, it requires each application to explicitly and statically specify the number of resources it will consume.
4) This static allocation is persistent and cannot be adjusted later. The impact is that can’t dynamically scale up or scale down the resources available to the cluster.


■ Standalone cluster consists of master and worker processes.  
■ A master process acts as the cluster manager: it accepts application request to be run and schedules worker resources (available CPU cores) among them. Worker processes launch executors (and the driver for applications in cluster-deploy mode) for task execution.  
■ Driver orchestrates and monitors execution of Spark jobs, and executors execute a job’s task.

■ Each Spark application would have its own set of executors and a separate driver running in the cluster

■ Spark standalone cluster running on two nodes with two workers:  
Step 1. A client process submits an application to the master.  
Step 2. The master instructs one of its workers to launch a driver.  
Step 3. The worker spawns a driver JVM.  
Step 4. The master instructs both workers to launch executors for the application.  
Step 5. The workers spawn executor JVMs.  
Step 6. The driver and executors communicate independent of the cluster’s processes.

■ A master process is the most important component in the standalone cluster. If the master process dies, the cluster becomes unusable: clients can’t submit new applications to the cluster, and users can’t see the state of the currently running ones.  
■ Worker processes, on the other hand, aren’t critical for cluster availability. If one of the workers becomes unavailable, Spark will restart its tasks on another worker.

■ Spark standalone provides two ways to recover application and worker data that was running before the master died: 
1. Filesystem: master persists information about registered workers and running applications in the directory specified by the spark.deploy.recoveryDirectory parameter.
If enabled, the master will restore worker state instantly (no need for them to re-register), along with the state of any running applications. You enable filesystem recovery by setting the spark.deploy.recoveryMode parameter to FILESYSTEM.
2. ZooKeeper: ZooKeeper provides both recovery and high-availability services for master processes. Master persist information about registered workers and running applications in ZooKeeper. 
To store the recovery data, ZooKeeper uses the directory specified by the spark.deploy.zookeeper.dir parameter on the machines where ZooKeeper is running.
You turn on ZooKeeper recovery by setting the spark.deploy.recoveryMode parameter to ZOOKEEPER

■ Submitting an application in cluster-deploy mode, --supervise option tells Spark to restart the driver process if it fails (or ends abnormally). This restarts the entire application because it isn’t possible to recover the state of a Spark program and continue at the point where it failed.


# Hadoop YARN

Source: Spark in Action, cap 12

YARN is Hadoop’s resource manager and execution system. Running Spark on YARN has several advantages:

■ YARN lets you run different types of Java applications, not just Spark, so you can mix legacy Hadoop and Spark applications with ease.  
■ YARN provides methods for isolating and prioritizing applications among users and organizations, functionality the standalone cluster doesn’t have.  
■ You don’t have to install Spark on all nodes in the cluster.  
■ It can run and dynamically distribute resources between different applications - some running Hive, some using HBase or Impala, and others running Spark.

Limitations:
1) YARN is not a standalone component. It is cluster manager for Hadoop, and as such it has always been closely tied to the Hadoop ecosystem.
2) There are more pieces involved in standing up and maintaining a YARN cluster, introducing more points of failure and potential challenges
3) Spark jobs running on YARN start slower than on Standalone cluster.


■ YARN main components are a resource manager (similar to Spark master) for the cluster and node managers (similar to Spark workers) for each node in the cluster.  
■ Applications on YARN run in containers (JVM processes to which CPU and memory resources are granted).  
■ An application master, which runs the application, runs in its own container, and it is responsible for requesting application resources for the resource manager.  
■ When Spark is running on YARN, the Spark driver process acts as the YARN application master. Node managers track resources used by containers and report to the resource manager.

■ Hadoop YARN cluster running on two nodes with two node managers:  
Step 1. A client process submits an application to the resource manager.  
Step 2. The resource manager instructs one of its node managers to allocate a container for the application master.  
Step 3. A node manager launches a container for the application master (Spark driver)  
Step 4. The application master asks the resource manager for more containers to be used as Spark executors.  
Step 5. If granted, application master asks the node managers to launch executors in the new containers.  
Step 6. The driver and executors communicate independent of the cluster’s processes.

■ YARN’s ResourceManager has a pluggable interface that allows different plug-ins to implement its resource-scheduling functions. The ones available are : FIFO scheduler, capacity scheduler, and fair scheduler.  
1. FIFO scheduler: it lets applications take all the resources they need. If two applications require the same resources, the first
application that requests will be first served.
2. Capacity scheduler: it was designed for sharing of a single YARN cluster by different organizations, and it guarantees that each organization will always have a certain amount of resources available (guaranteed capacity) defined by a queue. Each queue’s capacity determines the percentage of cluster resources that can be used by applications submitted to it. A hierarchy of queues can be set up, so that sub-queues (sub-organizations) can share the resources of a single queue.
3. Fair scheduler: it tries to assign resources so that all applications get (on average) an equal share. Like the capacity scheduler, it also organizes applications into queues. The fair scheduler also supports application priorities (some applications should get more resources than others) and minimum capacity requirements. It also enables preemption, meaning when an application
demands resources, the fair scheduler can take some resources from other running applications.