
## Main Streaming Operations

1) create streaming dataframe
```
val df = spark.readStream.format("<type>")
  .option("key","value")
  .schema(schema)
  .load()
```

2) write result to data sink
```
df.writeStream.format("<type>")
  .outputMode("<mode>")
  .trigger(Trigger.ProcessingTime("<time>"))
  .option("checkpointLocation", checkpointDir)
  .start()
			   
df.writeStream
  .foreachBatch(foreachbatchWriter _)
  .outputMode("<mode>")
  .option("checkpointLocation", checkpointDir)
  .start()
 
df.writeStream.foreach(foreachWriter _).start()
```

3) handling event-time events
```
df.withWatermark("eventTime", "<time>")
  .groupBy("column", window("eventTime", "<window-interval>", "<slide-interval>"))
  .mean("value")
```