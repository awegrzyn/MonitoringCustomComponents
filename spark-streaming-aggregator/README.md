# Spark Streaming aggregator

## Description

Tested on: 
- Spark 2.2.0
- Scala 2.11
- Flume 1.7.0

## Build
1. Install Apache Maven - https://maven.apache.org/install.html
2. Clone repository
 ~~~
 git clone https://github.com/AliceO2Group/MonitoringCustomComponents.git && cd spark-streaming-aggregator  
 ~~~
3. Compile
 ~~~
 mvn clean -e install -DskipTests
 ~~~
## Run
1. Move created `.jar` file from `target/` to Spark's `jars/` directory
2. Configure a Flume agent having a Avro Sink 

*Example:*
 ~~~
agent.sources = my_source
agent.sources.my_source.channels = channel_mem

agent.channels = channel_mem
agent.channels.channel_mem.type = memory

agent.sinks = avro_sink
agent.sinks.avro_sink.type = avro
agent.sinks.avro_sink.channel = channel_mem
agent.sinks.avro_sink.hostname = <receiver_host>
agent.sinks.avro_sink.port = <receiver_port>
 ~~~
3. Insert in the Spark's `jars/` directory the Flume dependencies:
- `spark-streaming-flume-assembly_2.11-2.2.0.jar`
- `spark-streaming-flume_2.11-2.2.0.jar`

They could be downloaded from [here](https://search.maven.org/#search%7Cga%7C1%7Cspark%20flume)

4. Submit the Spark Streaming Job
 ~~~
 $SPARK_HOME/bin/spark-submit --class ch.cern.alice.o2.spark.streaming.SparkStreamingAggregator \
  --master local[*] $SPARK_HOME/jars/spark-streaming-aggregator-1.0-SNAPSHOT.jar \ 
  <receiver_host> <receiver_port> <db_host> <db_port>

 ~~~
