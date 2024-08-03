Apache Spark working directories

|Directory contents   | Default location | Environment variable | Environment variable |
|---------------------|------------------|----------------------|----------------------|
|Log files            | $SPARK_HOME/logs | SPARK_LOG_DIR        | yarn.nodemanager.local-dirs (yarn-site.xml) = $HADOOP_HOME/logs/userlogs |
|Working data for the worker process   | $SPARK_HOME/work | SPARK_WORKER_DIR     |  |
|Shuffle and RDD data | /tmp             | SPARK_LOCAL_DIRS     | hadoop.tmp.dir (core-site.xml) = ./nm-local-dir/usercache/root/appcache/application_xxxxxxxxxxx_xxxxxxx/blockmgr-_xxxxxxxxxxx_xxxxxxx |
| PID files           | /tmp             | SPARK_PID_DIR        |  |
