---
title: 'Python - Get no of messages from MSK'
date: 2023-08-06T12:33:46+10:00
author: Mohan
draft: false
weight: 2
heroHeading: 'Python - Get message counts for each topic based on offset from MSK'
heroSubHeading: ''
heroBackground: 'https://source.unsplash.com/ieic5Tq8YMk/1600x400'
thumbnail: 'https://source.unsplash.com/ieic5Tq8YMk/600x335'
#images: ['https://source.unsplash.com/random/400x600/?nature',
#'https://source.unsplash.com/random/400x300/?travel','https://source.unsplash.com/random/400x300/?architecture','https://source.unsplash.com/random/400x600/?buildings',#'https://source.unsplash.com/random/400x300/?city','https://source.unsplash.com/random/400x600/?business']
---

Apache Kafka is a powerful distributed event streaming platform that often involves monitoring and managing the data in various partitions of a topic. In this article, we'll explore how to use Python to determine the number of messages in each partition of a Kafka topic.

## Prerequisites

Before you start, make sure you have the following:

1. A running Kafka cluster.
2. Python installed on your system.
3. Confluent's `confluent-kafka-python` library, which provides a Kafka client for Python.

## Installing the Required Libraries

First, install the `confluent-kafka-python` library, which is a Python client for Apache Kafka:

```bash
pip install confluent-kafka

```

## Listing the Number of Messages in Kafka Partitions with Python

Here's a Python script that demonstrates how to list the number of messages in each partition of a Kafka topic:

```python

from confluent_kafka import Consumer, KafkaError

conf = {
    'bootstrap.servers': 'kafka-broker:9092',  # Replace with the address of your Kafka broker
    'group.id': 'my-consumer-group',
    'auto.offset.reset': 'earliest'
}

consumer = Consumer(conf)

# Specify the Kafka topic for which you want to count messages
topic = 'my-topic'

# Query the metadata to get the number of partitions for the topic
metadata = consumer.list_topics(topic)
num_partitions = len(metadata.topics[topic].partitions)

# Function to fetch the current offset for each partition
def get_offsets():
    offsets = {}
    for partition in range(num_partitions):
        offsets[partition] = consumer.position(consumer.assignment()[partition])
    return offsets

try:
    # Subscribe to the Kafka topic
    consumer.subscribe([topic])

    # Start consuming messages to update offsets
    while True:
        msg = consumer.poll(1.0)
        if msg is None:
            offsets = get_offsets()
            print(f'Number of messages in each partition for topic {topic}:')
            for partition, offset in offsets.items():
                print(f'Partition {partition}: {offset} messages')
            break
        if msg.error():
            if msg.error().code() == KafkaError._PARTITION_EOF:
                continue
            else:
                print(msg.error())
        else:
            pass

except KeyboardInterrupt:
    pass

finally:
    consumer.close()
```

Make sure to replace the placeholders (kafka-broker, my-topic, etc.) with the appropriate values for your Kafka setup and the topic you want to analyze.

## Conclusion

In this article, we've demonstrated how to use Python and the confluent-kafka-python library to list the number of messages in each partition of a Kafka topic. This approach can be useful for monitoring and managing data distribution within Kafka topics, ensuring that you have an accurate count of messages in each partition.


