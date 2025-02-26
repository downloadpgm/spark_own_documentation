
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

----------

// Spark, The Definite Guide, Cap 11, pag 204

When you use the Dataset API, for every row it touches, Spark converts the internal Row format to the object you specified (a case class or Java class). This conversion slows down your operations but can provide more flexibility.

// Beginning Apache Spark, Cap 4, pag 134

When you use the strongly typed Dataset APIs, Spark implicitly converts each Row instance to the domain-specific object that you provide. This conversion has some cost in terms of performance; however, it provides more flexibility.


Mechanism Implemented : (Spark, The Definite Guide, Cap 11, pag 204)

For example, given a class Person with two fields, name (string) and age (int), an encoder directs
Spark to generate code at runtime to serialize the Person object into a binary structure. When using
DataFrames or the “standard” Structured APIs, this binary structure will be a Row. When we want to
create our own domain-specific objects, we specify a case class in Scala or a JavaBean in Java.

Spark will allow us to manipulate this object (in place of a Row) in a distributed manner.
When you use the Dataset API, for every row it touches, this domain specifies type, Spark converts
the Spark Row format to the object you specified (a case class or Java class). This conversion slows
down your operations but can provide more flexibility.