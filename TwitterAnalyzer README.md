
README
Twitter Analyzer
Please make sure the environment is set.
ref: 
1. java : jdk-8-64bit
2. scala: 2.11.12
3. spark: 2.4.5

Step 1: Download the code
Download the 2.5.0 release and un-tar it.
	> tar -xzf kafka_2.12-2.5.0.tgz
	> cd kafka_2.12-2.5.0


Step 2: Start the server
> bin/zookeeper-server-start.sh config/zookeeper.properties

Now start the Kafka server:
> bin/kafka-server-start.sh config/server.properties


Step 3: Create a topic
Create topic named "test"
bin/kafka-topics.sh --create --bootstrap-server localhost:9092 --replication-factor 1 --partitions 1 --topic test


Step 4: Start producer
Run the producer.
> bin/kafka-console-producer.sh --bootstrap-server localhost:9092 --topic test


Step 5: Start a consumer

> bin/kafka-console-consumer.sh --bootstrap-server localhost:9092 --topic test


Step 6: sbt assembly
Go to the project root location and run(may need to switch to adiministrator/root user):
> sbt assembly


Step 7: run spark-submit(may need to switch back to non-root user)
>spark-submit --packages org.apache.bahir:spark-streaming-twitter_2.11:2.3.4,\org.apache.spark:spark-sql-kafka-0-10_2.11:2.4.0 --class TwitterSentimentAnalyzer "PathtoJarFile" test


Step 8: Run elasticsearch and kibana and logstash
1. Elasticsearch using the following command in the elasticsearch-5.6.8/bin directory:
./elasticsearch
2. Kibana using the following command in the kibana-5.6.8-darwin-x86_64/bin directory:
./kibana
3. Logstash: Go to the logstash-5.6.8 directory and create a file logstash-simple.conf with following
content:
input {
kafka {
bootstrap_servers => "localhost:9092"
topics => ["YourTopic"]
}
}
output {
elasticsearch {
hosts => ["localhost:9200"]
index => "YourTopic-index"
}
}
Then run the following command
bin/logstash -f logstash-simple.conf


Step 9: go to kibana to check and visualize the data








