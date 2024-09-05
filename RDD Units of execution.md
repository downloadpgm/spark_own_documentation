
## Units of Physical Execution

Jobs: work required to compute RDD in runJob  
Stages: a wave of work within a job, corresponding to one or more pipelined RDD´s  
Tasks: a unit of work within a stage, corresponding to one RDD partition  
Shuffle : the transfer of data between stages


## Workflow Spark application :

1. Create some input RDDs from external data. 
2. Transform them to define new RDDs using transformations like filter().
3. Ask Spark to persist() any intermediate RDDs that will need to be reused.
4. Launch actions such as count() and first() to kick off a parallel computation.


## Stage/Task steps:

A physical stage will launch tasks that each do the same thing but on specific parti
tions of data. Each task internally performs the same steps:

1. Fetching its input, either from data storage (if the RDD is an input RDD), an
existing RDD (if the stage is based on already cached data), or shuffle outputs.

2. Performing the operation necessary to compute RDD(s) that it represents. For
instance, executing filter() or map() functions on the input data, or perform
ing grouping or reduction.

3. Writing output to a shuffle, to external storage, or back to the driver (if it is the
final RDD of an action such as count())


## Job/DAG steps:

To summarize, the following phases occur during Spark execution:

- User code defines a DAG (directed acyclic graph) of RDDs

Operations on RDDs create new RDDs that refer back to their parents, thereby
creating a graph.

- Actions force translation of the DAG to an execution plan

When you call an action on an RDD it must be computed. This requires comput
ing its parent RDDs as well. Spark’s scheduler submits a job to compute all
needed RDDs. That job will have one or more stages, which are parallel waves of
computation composed of tasks. Each stage will correspond to one or more
RDDs in the DAG. A single stage can correspond to multiple RDDs due to
pipelining.

- Tasks are scheduled and executed on a cluster

Stages are processed in order, with individual tasks launching to compute seg
ments of the RDD. Once the final stage is finished in a job, the action is com
plete.

In a given Spark application, this entire sequence of steps may occur many times in a
continuous fashion as new RDDs are created.