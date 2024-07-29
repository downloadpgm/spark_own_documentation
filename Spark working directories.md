Apache Spark working directories

|Directory contents   | Default location | Environment variable |
|---------------------|------------------|----------------------|
|Log files            | $SPARK_HOME/logs | SPARK_LOG_DIR        |
|Working data for     |                  |                      |
|the worker process   | $SPARK_HOME/work | SPARK_WORKER_DIR     |
|Shuffle and RDD data | /tmp             | SPARK_LOCAL_DIRS     |
| PID files           | /tmp             | SPARK_PID_DIR        |
