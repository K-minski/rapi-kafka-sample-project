# Kafka Quick Test Enviroment Setup

### Description 
Main purpose for this repository is to set it up quickly for local proof of principle tests. 
Additionally, kafka-ui is included for easier tinkering

Main purpose for this repository to set Kafka Broker, Zookeeper, Schema Registry for local proof of principle tests with help of docker / docker compose either on Linux (Ubuntu) or Windows (WSL2). On guide how to set up those, please follow official Docker Documentation: https://docs.docker.com/engine/install/

### How to Run
1. Clone repository:
```
git clone https://github.com/K-minski/kafka-test-env.git
```
2. Simply use below command in project folder:
```
docker compose up
```

### Open default endpoints

| Service         | Container name  | Endpoint & Port |
|-----------------|-----------------|-----------------|
| Kafka Broker    | broker0         | localhost:9092  |
| Schema-Registry | schemaregistry  | localhost:8085  |
| Kafka-UI        | kafka-ui        | localhost:8081  |


### Kafka Avro Schema for key:value pairs:

Navigate to Kafka-UI and Register Schemas using following subjects (Inside ExampleSchema)
#### example.avro.topic-value => example-value.avsc
```
	curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
    --data '{"schema": "{ \"namespace\": \"example.avroSchema\", \"type\": \"record\", \"name\": \"ExampleMessage\", \"fields\": [ {\"name\": \"name\", \"type\": \"string\"}, {\"name\": \"favorite_city\",  \"type\": [\"string\", \"null\"]}, {\"name\": \"favorite_number\",  \"type\": [\"int\", \"null\"]}, {\"name\": \"favorite_color\", \"type\": [\"string\", \"null\"]} ] }" }' \
    http://localhost:8085/subjects/example.avroSchema.topic-value/versions
```

#### example.avro.topic-key => example-key.avsc
```
	curl -X POST -H "Content-Type: application/vnd.schemaregistry.v1+json" \
  	--data '{"schema": "{ \"namespace\": \"example.avroSchema\", \"type\": \"record\", \"name\": \"ExampleKey\", \"fields\": [ {\"name\": \"key\", \"type\": \"string\"} ] }"}' \
  	http://localhost:8085/subjects/example.avroSchema.topic-key/versions
```

### Other helpful commands
Initialize Setup:
```
docker compose -f docker-compose.yaml up
```
Destroy(Delete) initialized Setup:
```
docker compose -f docker-compose.yaml down
```
Start initialiezed Setup (Only if terminated earlier):
```
docker compose -f docker-compose.yaml start
```
Terminate and stop initialiezed Setup:
```
docker compose -f docker-compose.yaml stop
```
Enter container’s (<containers name>) internal CLI:
```
docker exec -it $(docker ps | grep <container name> | cut -d" " -f 1) /bin/sh
```
