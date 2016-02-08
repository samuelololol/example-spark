reference from: [http://blog.antlypls.com/blog/2015/10/05/getting-started-with-spark-streaming-using-docker/](http://blog.antlypls.com/blog/2015/10/05/getting-started-with-spark-streaming-using-docker/)


### create project

```
$ ./sbt_init.sh demo
```

### setting assembly file for more plugins(ie. com.eed3si9n)

```
$ cd demo
$ vi project/assembly.sbt

addSbtPlugin("com.eed3si9n" % "sbt-assembly" % "0.13.0")

```

### edit source code

```
$ vim demo/src/main/scala/com/...
```

### build

```
$ cd demo/
$ sbt assembly
```

### start spark

```
$ cd demo/
$ docker-compose run --rm spark
```

### create topic and key-in words(using samuelololol/kafka)

```
$ /opt/kafka_2.10-0.8.2.0/bin/kafka-topics.sh --create --zookeeper zk1 --replication-factor 1 --partitions 1 --topic word-count
$ /opt/kafka_2.10-0.8.2.0/bin/kafka-console-producer.sh --broker-list 172.17.0.5:9092 --topic word-count

Hello World!!!

```

### submit app

```
bash-4.1#   spark-submit \
            --master yarn-client \
            --driver-java-options \
              "-Dlog4j.configuration=file:///app/log4j.properties" \
            --class \
              com.example.spark.DirectKafkaWordCount app/demo.jar \
              kafka:9092 \
              word-count
```

