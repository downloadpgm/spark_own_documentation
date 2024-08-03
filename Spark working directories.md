Apache Spark working directories

|Directory contents   | Default location | Environment variable (Standalone) |
|---------------------|------------------|----------------------|
|Log files            | $SPARK_HOME/logs | SPARK_LOG_DIR        |
|Working data for the worker process   | $SPARK_HOME/work | SPARK_WORKER_DIR     |
|Shuffle and RDD data | /tmp             | SPARK_LOCAL_DIRS     |
| PID files           | /tmp             | SPARK_PID_DIR        |


|Directory contents   | Default location | Environment variable (YARN) |
|---------------------|------------------|----------------------|
|Log files            | $HADOOP_HOME/logs/userlogs | yarn.nodemanager.local-dirs (yarn-site.xml)
|Working data for the worker process   | $SPARK_HOME/work | SPARK_WORKER_DIR     |
|Shuffle and RDD data | $HADOOP_TMP_DIR/nm-local-dir/usercache/root/appcache/application_xxxxxxxxxxx_xxxxxxx/blockmgr-_xxxxxxxxxxx_xxxxxxx | hadoop.tmp.dir (core-site.xml) |
| PID files           | /tmp             | SPARK_PID_DIR        |