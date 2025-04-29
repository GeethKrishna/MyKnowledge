### Zookeeper:
#### zookeeper.properties file:
```
dataDir=C:\kafka/tmp/zookeeper

clientPort=2181

maxClientCnxns=0

admin.enableServer=false

authProvider.1=org.apache.zookeeper.server.auth.SASLAuthenticationProvider

requireClientAuthScheme=sasl
```
#### zookeeper_jass.conf file:

```
Server {

    org.apache.zookeeper.server.auth.DigestLoginModule required

    user_super="admin-secret";

};
```
#### commands: 
```
1.
$env:KAFKA_OPTS="-Djava.security.auth.login.config=C:\kafka\config\zookeeper_jaas.conf"

2.
.\bin\windows\zookeeper-server-start.bat .\config\zookeeper.properties
```
### Kafka server:
#### server.properties file:
```
broker.id=0

num.network.threads=3

num.io.threads=8

socket.send.buffer.bytes=102400

socket.receive.buffer.bytes=102400

socket.request.max.bytes=104857600

log.dirs=C:/kafka/tmp/kafka-logs

num.partitions=1

num.recovery.threads.per.data.dir=1

offsets.topic.replication.factor=1

transaction.state.log.replication.factor=1

transaction.state.log.min.isr=1

log.retention.hours=168

log.retention.check.interval.ms=300000

zookeeper.connect=localhost:2181

zookeeper.connection.timeout.ms=18000

group.initial.rebalance.delay.ms=0

listeners=SASL_PLAINTEXT://0.0.0.0:9092

advertised.listeners=SASL_PLAINTEXT://localhost:9092

security.inter.broker.protocol=SASL_PLAINTEXT

sasl.mechanism.inter.broker.protocol=PLAIN

sasl.enabled.mechanisms=PLAIN

authorizer.class.name=

allow.everyone.if.no.acl.found=false

zookeeper.sasl.enabled=true

zookeeper.sasl.client=true

zookeeper.sasl.client.username=super

zookeeper.sasl.client.password=admin-secret
```
#### kafka_jaas.conf file:
```
KafkaServer {

    org.apache.kafka.common.security.plain.PlainLoginModule required

    username="super"

    password="admin-secret"

    user_super="admin-secret";

};

  

Client {

    org.apache.zookeeper.server.auth.DigestLoginModule required

    username="super"

    password="admin-secret";

};
```
#### commands
```
1.
$env:KAFKA_OPTS="-Djava.security.auth.login.config=C:\kafka\config\kafka_jaas.conf"

2.
 .\bin\windows\kafka-server-start.bat .\config\server.properties
```

command to give all permissions to "super":
client.properties:
```
security.protocol=SASL_PLAINTEXT
sasl.mechanism=PLAIN
```

kafka_client_jass.conf:
```
KafkaClient {
    org.apache.kafka.common.security.plain.PlainLoginModule required
    username="super"
    password="admin-secret";

};
```

commands:
```
$env:KAFKA_OPTS="-Djava.security.auth.login.config=C:\kafka\config\kafka_client_jaas.conf"

bin\windows\kafka-acls.bat --bootstrap-server localhost:9092 --add --allow-principal "User:super" --operation Create --operation Describe --operation Write --topic "*" --command-config C:\kafka\config\client.properties
```