
SchemaRDD (1.0)
- RDD of Row objects with additional schema information
- use RDD API to manipulate rows of data
- SQL available if SchemaRDD registered as temporary table (registered with registerTempTable) with hiveCtx.sql()
- access a field by index using standard get() or getType() methods.

Dataframe (1.3) 
- use DSL/SQL to manipulate rows of data
- run-time type checking; uses schema to map JVM types to/from Spark types
- can access functional operators if convert to RDD[Row] objects  
- SQL available if Dataframe registered as temp table (createOrReplaceTempView()) / permanent table (write.saveAsTable())
				
Dataset (1.6) 
- use DSL/SQL to manipulate Class objects
- compile-time type checking; dismiss a schema once Class validates field types 
- natively use functional operators to manipulate Class objects
- SQL available if Dataframe registered as temp table (createOrReplaceTempView()) / permanent table (write.saveAsTable())

----------

Row object - internally represented by array of bytes
           - array of bytes interface not available to users
		   - use get() or getType() to access specific column

Dataframe - wraps a RDD containing objects of Row type
          - has an appended schema defining column names and data types
		  - enforced only when job is run (runtime)
		  
Dataset - wraps a RDD containing objects of specific class
        - dismiss a schema; column names and data types enforced by case class
		- enforced when dataset created