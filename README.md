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