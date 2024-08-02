## Starting Spark standalone manually:
 
On master:  
bin/spark-class org.apache.spark.deploy.master.Master

Then on workers:  
bin/spark-class org.apache.spark.deploy.worker.Worker spark://masternode:7077


## Starting Spark standalone using launch scripts:

1) On master:  
   run ssh-keygen accepting default options.  
   ssh-keygen -t dsa

   On workers:  
   copy ~/.ssh/id_dsa.pub from your master to the worker  
   cat ~/.ssh/id_dsa.pub >> ~/.ssh/authorized_keys  
   chmod 644 ~/.ssh/authorized_keys

3) edit conf/slaves file on your master and fill in the workers’ hostnames

4) run sbin/start-all.sh on your master

--------------

## Connecting Spark to YARN cluster

1) copy yarn-site.xml to $SPARK_HOME/conf

2) copy also core-site.xml to $SPARK_HOME/conf

3) set HADOOP_CONF_DIR = $SPARK_HOME/conf

4) run spark with --master yarn flag

--------------

## Connecting Spark SQL to Hive metastore:

1) copy hive-site.xml to $SPARK_HOME/conf

2) copy also core-site.xml and hdfs-site.xml to $SPARK_HOME/conf

Executing Spark SQL without Hive installation

1) Spark SQL will create a local metastore_db dir in local directory

2) Any tables created using HiveQL’s CREATE TABLE will be placed in the /user/hive/warehouse directory on your default filesystem (either your local filesystem, or HDFS if you have a hdfs-site.xml on your classpath).

## Configuring a Remote Hive metastore

1) configure hive-site.xml file to access a remote Hive metastore setting following properties :

■ javax.jdo.option.ConnectionURL - JDBC connection URL  
■ javax.jdo.option.ConnectionDriverName - Class name of the JDBC driver  
■ javax.jdo.option.ConnectionUserName - Database username  
■ javax.jdo.option.ConnectionPassword - Database user password

2) Initialize the metastore database and create the necessary tables running Hive’s schematool.

---------------

## Starting Spark Thriftserver :

1) copy hive-site.xml to $SPARK_HOME/conf

2) copy also core-site.xml and hdfs-site.xml to $SPARK_HOME/conf
   
3) start jdbc server  
   ./sbin/start-thriftserver.sh --master sparkMaster
   
4) connect to it via beeline  
   ./bin/beeline -u jdbc:hive2://localhost:10000

