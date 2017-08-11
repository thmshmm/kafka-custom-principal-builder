# Apache Kafka - Custom Principal Builder
A custom principal builder for Apache Kafka using TLS authentication.

The principal builder demonstrates how to extract specific details from a TLS certificate and use it as custom string for user authentication.

Example certificate:

    Subject: C=US, ST=Ca, L=PaloAlto, O=CONFLUENT, OU=TEST, CN=consumer.test.confluent.io

The principal builder will extract the CN property from the certificate and generate the principal "**User:consumer.test.confluent.io**" instead of the default "**User:CN=consumer.test.confluent.io,OU=TEST,O=CONFLUENT,L=PaloAlto,ST=Ca,C=US**".

## Build the project
```
mvn package
```
This creates the file "target/kafka-custom-principal-builder-1.0-SNAPSHOT.jar" which needs to be added to the JVM classpath.

## Prerequisites
- Configure Kafka brokers and clients to use TLS authentication
- Create a topic "sometopic"
- Create a directory "/some/path" and place the generated the jar into it

## Configure Kafka

### Set JVM classpath
    export CLASSPATH=/some/path/*

### Kafka properties
    principal.builder.class=de.thmshmm.kafka.CustomPrincipalBuilder
    ssl.client.auth=required

## Test
Set ACL's on the topic "sometopic" for principal "**User:consumer.test.confluent.io**" and read/write messages with a consumer/producer.
