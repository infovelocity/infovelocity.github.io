---
title: 'Python - Consuming from MSK with TLS'
date: 2023-08-05T12:33:46+10:00
author: Mohan
draft: false
weight: 2
heroHeading: 'Consuming Kafka Messages with Python using TLS'
heroSubHeading: ''
heroBackground: 'https://source.unsplash.com/95YRwf6CNw8/1600x400'
thumbnail: 'https://source.unsplash.com/95YRwf6CNw8/600x335'
#images: ['https://source.unsplash.com/random/400x600/?nature',
#'https://source.unsplash.com/random/400x300/?travel','https://source.unsplash.com/random/400x300/?architecture','https://source.unsplash.com/random/400x600/?buildings',#'https://source.unsplash.com/random/400x300/?city','https://source.unsplash.com/random/400x600/?business']
---


Apache Kafka is a popular choice for building scalable and robust real-time data pipelines. When dealing with sensitive data or communication across untrusted networks, it's crucial to secure your Kafka communication using TLS. In this article, we'll explore how to consume Kafka messages with Python while utilizing TLS for secure data transfer.

## Prerequisites

Before you start, make sure you have the following:

1. A running Kafka cluster with TLS enabled.
2. Python installed on your system.
3. Confluent's `confluent-kafka-python` library, which provides a Kafka client for Python.

## Installing the Required Libraries

First, install the `confluent-kafka-python` library, which is a Python client for Apache Kafka:

```bash
pip install confluent-kafka

```

## Consuming Kafka Messages with Python and TLS

Here's a basic Python script that demonstrates how to consume messages from a Kafka topic using TLS for secure communication:


```bash
from confluent_kafka import Consumer, KafkaError

conf = {
    'bootstrap.servers': 'kafka-broker:9093',  # Replace with the address of your Kafka broker
    'group.id': 'my-consumer-group',
    'security.protocol': 'ssl',
    'ssl.ca.location': '/path/to/ca-cert',  # Path to your CA certificate file
    'ssl.certificate.location': '/path/to/client-cert',  # Path to your client certificate file
    'ssl.key.location': '/path/to/client-key',  # Path to your client private key file
    'auto.offset.reset': 'earliest'
}

consumer = Consumer(conf)

# Subscribe to the Kafka topic
consumer.subscribe(['my-topic'])

try:
    while True:
        msg = consumer.poll(1.0)

        if msg is None:
            continue
        if msg.error():
            if msg.error().code() == KafkaError._PARTITION_EOF:
                continue
            else:
                print(msg.error())
        else:
            print('Received message: {}'.format(msg.value().decode('utf-8')))

except KeyboardInterrupt:
    pass

finally:
    consumer.close()

```

Make sure to replace the placeholders (kafka-broker, file paths, etc.) with the appropriate values for your Kafka setup.

## Conclusion

In this article, we've explored how to consume Kafka messages with Python using TLS for secure communication. By utilizing the confluent-kafka-python library and configuring the appropriate security properties, you can ensure that your Kafka communication is encrypted and secure, making it suitable for handling sensitive data or communication across untrusted networks.


