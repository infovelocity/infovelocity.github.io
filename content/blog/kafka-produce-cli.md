---
title: 'Kafka - produce messages via cli'
date: 2023-08-02T09:30:00+10:00
author: Mian
draft: false
weight: 2
heroHeading: 'Producing Messages to Kafka via CLI'
heroSubHeading: ''
heroBackground: 'blog/kafka-white.png'
thumbnail: 'blog/kafka-black.png'
#images: ['https://source.unsplash.com/random/400x600/?nature',
#'https://source.unsplash.com/random/400x300/?travel','https://source.unsplash.com/random/400x300/?architecture','https://source.unsplash.com/random/400x600/?buildings',#'https://source.unsplash.com/random/400x300/?city','https://source.unsplash.com/random/400x600/?business']
---

Apache Kafka is a versatile platform for handling real-time data streams. One essential task is producing messages to Kafka topics. In this article, we'll explore how to produce Kafka messages using the command-line interface (CLI).

## Prerequisites

Before you start, make sure you have the following:

1. A running Kafka cluster.
2. Kafka command-line tools installed on your system (part of the Kafka distribution).

## Using the Kafka Console Producer

The Kafka distribution comes with a powerful command-line tool called `kafka-console-producer` that allows you to produce messages to Kafka topics. Here's how you can use it:

```bash
kafka-console-producer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name>

```

* `<kafka-broker>:<port>`: Replace this with the address of your Kafka broker and the port it's running on.
* `<topic-name>`: Specify the Kafka topic you want to produce messages to.

Once you run this command, you can start typing messages, and each line you enter will be treated as a separate message.
Producing Messages with a Key

Kafka messages can have both a key and a value. You can produce messages with a key using the
`--property "parse.key=true" --property "key.separator=:"` flags.

```sh

kafka-console-producer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name> --property "parse.key=true" --property "key.separator=:"

```

Now, when you enter messages, you can specify a key for each message by separating it with a colon (":").

## Producing Messages with Avro Serialization

If your Kafka topic uses Avro serialization for messages, you can produce and serialize Avro messages using the --property value.serializer=io.confluent.kafka.serializers.KafkaAvroSerializer properties:

```bash

kafka-console-producer.sh --bootstrap-server <kafka-broker>:<port> --topic <topic-name> --property value.serializer=io.confluent.kafka.serializers.KafkaAvroSerializer --property schema.registry.url=<schema-registry-url>

```
* `<schema-registry-url>`: Replace this with the URL of your Avro schema registry.

## Conclusion

In this article, we've covered the essentials of producing messages to Kafka via the command-line interface (CLI). This approach is useful for quickly testing and sending messages to Kafka topics without writing custom producer code. The kafka-console-producer tool is a valuable utility that every Kafka developer should be familiar with.