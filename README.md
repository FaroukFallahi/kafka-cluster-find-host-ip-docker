# Kafka cluster finding hosts ip 
new SRE team member assignment:
1. Prepare two tiny python scripts that:
    - [X] The first script finds the host's public IP address (with the help of third-party public APIs) and sends the result to the Kafka every 5 seconds.
    - [X] The second one fetches the last data from that Kafka, and prints the IP every 5 seconds.
2. Prepare a docker-compose.yml file that contains 8 containers:
    - [X] 3-replica Kafka cluster.
    - [X] 3-replica ZooKeeper cluster.
    - [X] These two cluster nodes have connectivity in an internal docker network.
    - [X] The kafka nodes have two listeners: External, and Internal for inter-cluster connections.
    - [X] External listener ports should be exposed on the host OS.
    - [X] 2 containers that run your scripts within the compose network.

# Python scripts
## Producer.py
### This script will get the host's public ip(from third-party api) and send it to kafka-cluster every 5 sec.
configs that should pass it as environment variable:
- `KAFKA_SERVERS` -- a list of kafka IPs with port like `kafka1.server:9092,kafka2.server:9092`
- `KAFKA_TOPIC` -- the kafka topic
- `IP_WEB_API` (optional-default: `https://api.ipify.org`) -- api that find host's public api. 
- `logLevel` (optional-default: `WARNING`)-- logging level.

## consumer.py
### This script will get the messages from kafka-cluster and print it.
configs that should pass it as environment variable:
- `KAFKA_SERVERS` -- a list of kafka IPs with port like `kafka1.server:9092,kafka2.server:9092`
- `KAFKA_TOPIC` -- the kafka topic
- `AUTO_OFFSET_RESET` (optional-default: `latest`)-- Define the behavior of the consumer when there is no committed position or when an offset is out of range. You can choose either to reset the position to the `earliest` offset or the `latest` offset. 
- `logLevel` (optional-default: `WARNING`) -- logging level. 

# Docker-compose.yml

### When `up` this docker-compoer will have 8 container:
- `kafka-net` is a bridge network that connects our containers. 
- 3 zookeeper(`zoo1, zoo2, zoo3`) that will serves in  `zoo1:2181, zoo2:2182, zoo3:2183` addresses.
- 3 kafka (`kfk1, kfk2, kfk2`) that:
    - serve in `kafka-net` network with this addresses:
        - `kfk1:9092` -- `kfk1`
        - `kfk2:9092` -- `kfk2`
        - `kfk3:9092` -- `kfk3`
    - and they also are available in host's with this addresses:
        - `localhost:9093` -- `kfk1`
        - `localhost:9094` -- `kfk2`
        - `localhost:9095` -- `kfk3`
- A producer container will created from producer.py that accepts kafka's inner addresses as kafka servers and sends host's public ip to them.
- A consumer container will created from consumer.py that accepts kafka's inner addresses as kafka servers and receives data from them.
