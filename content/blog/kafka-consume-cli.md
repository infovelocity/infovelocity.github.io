---
title: 'Kafka - Consume messages via cli'
date: 2023-08-03T12:33:46+10:00
author: Mian
draft: false
weight: 2
heroHeading: 'Consuming Kafka Messages via CLI'
heroSubHeading: ''
heroBackground: 'blog/kafka-banner.png'
#heroBackground: 'https://source.unsplash.com/95YRwf6CNw8/1600x400'
thumbnail: 'blog/kafka-white.png'
#images: ['https://source.unsplash.com/random/400x600/?nature',
#'https://source.unsplash.com/random/400x300/?travel','https://source.unsplash.com/random/400x300/?architecture','https://source.unsplash.com/random/400x600/?buildings',#'https://source.unsplash.com/random/400x300/?city','https://source.unsplash.com/random/400x600/?business']
---

Apache Kafka is a powerful platform for handling real-time data streams. One of the most common tasks when working with Kafka is consuming messages from topics. In this article, we'll explore how to consume Kafka messages using the command-line interface (CLI).

## Prerequisites

Before you get started, make sure you have the following:

1. A running Kafka cluster.
2. Kafka command-line tools installed on your system (part of the Kafka distribution).

## Using the Kafka Console Consumer

The Kafka distribution comes with a handy command-line tool called `kafka-console-consumer` that allows you to consume messages from Kafka topics. Here's how you can use it:

```sh

kafka-console-consumer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name> [--from-beginning]

```

* `<kafka-broker>:<port>`: Replace this with the address of your Kafka broker and the port it's running on.
* `<topic-name>`: Specify the Kafka topic you want to consume messages from.
* `--from-beginning (optional)`: Use this flag if you want to start consuming messages from the beginning of the topic.

## Consuming Messages with Key and Value

Kafka messages consist of a key and a value. You can consume messages along with their keys using the `--property print.key=true` flag.

```sh

kafka-console-consumer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name> --from-beginning --property print.key=true

```


## Consuming Messages with Avro Serialization

If your Kafka topic uses Avro serialization for messages, you can consume and deserialize Avro messages using the

`--from-beginning`

`--property value.deserializer=io.confluent.kafka.serializers.KafkaAvroDeserializer` properties.

```sh

kafka-console-consumer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name> --from-beginning --property value.deserializer=io.confluent.kafka.serializers.KafkaAvroDeserializer --property schema.registry.url=<schema-registry-url>

```

* `<schema-registry-url>`: Replace this with the URL of your Avro schema registry.

## Conclusion
In this article, we've covered the basics of consuming Kafka messages via the command-line interface (CLI). This approach is useful for quickly testing and debugging your Kafka topics without writing custom consumer code. The kafka-console-consumer tool is a powerful utility that every Kafka developer should be familiar with.

