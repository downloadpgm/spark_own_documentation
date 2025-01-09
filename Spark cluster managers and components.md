
A Spark cluster is a set of interconnected JVM processes, usually running in a distributed manner on different machines.

The main clusters that Spark runs on are Hadoop YARN, Apache Mesos, and Spark standalone.

# Spark Standalone

■ Standalone cluster is built specifically for Spark applications and only run Spark applications. 
■ A Spark standalone cluster provides faster job startup than those jobs running on YARN.
■ It’s relatively simple and efficient to setup and comes with Spark out of the box.
■ Spark has to be installed on all nodes in the cluster in order for them to be usable as slaves.

■ Standalone cluster consists of master and worker processes. 
■ A master process acts as the cluster manager: it accepts application request to be run and schedules worker resources (available CPU cores) among them. Worker processes launch executors (and the driver for applications in cluster-deploy mode) for task execution. 
Driver orchestrates and monitors execution of Spark jobs, and executors execute a job’s task.

■ Each Spark application would have its own set of executors and a separate driver running in the cluster

■ Spark standalone cluster running on two nodes with two workers:
Step 1. A client process submits an application to the master.
Step 2. The master instructs one of its workers to launch a driver.
Step 3. The worker spawns a driver JVM.
Step 4. The master instructs both workers to launch executors for the application. 
Step 5. The workers spawn executor JVMs.
Step 6. The driver and executors communicate independent of the cluster’s processes.


# Hadoop YARN

YARN is Hadoop’s resource manager and execution system. Running Spark on YARN has several advantages:

■ YARN lets you run different types of Java applications, not just Spark, so you can mix legacy Hadoop and Spark applications with ease. 
■ YARN provides methods for isolating and prioritizing applications among users and organizations, functionality the standalone cluster doesn’t have.
■ You don’t have to install Spark on all nodes in the cluster.