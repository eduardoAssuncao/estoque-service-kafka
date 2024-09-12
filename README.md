Comandos docker para compose
- docker-compose up -d
- docker-compose -f .\docker-compose-bootstrap-zoo.yaml up -d
  
docker-compose usando kafka-ui

version: '2'
services:
  kafka-ui:
    image: provectuslabs/kafka-ui
    container_name: kafka-ui
    ports:
      - "9001:8080"
    restart: always
    environment:
      - KAFKA_CLUSTERS_0_NAME=TCEMA Desenvolvimento
      - KAFKA_CLUSTERS_0_BOOTSTRAPSERVERS=10.0.1.194:9092
      - KAFKA_CLUSTERS_0_ZOOKEEPER=10.0.1.194:2181
      - KAFKA_CLUSTERS_1_NAME=localhost Desenvolvimento
      - KAFKA_CLUSTERS_1_BOOTSTRAPSERVERS=localhost:9092
      - KAFKA_CLUSTERS_1_ZOOKEEPER=localhost:2181


docker-compose completo com kafka e zookeeper

version: "3.3"

services:
  zookeeper:
    image: "docker.io/bitnami/zookeeper:latest"
    ports:
      - "2181:2181"
    volumes:
      - "zookeeper_data:/bitnami"
    environment:
      - ALLOW_ANONYMOUS_LOGIN=yes

  kafka:
    image: "docker.io/bitnami/kafka:latest"
    ports:
      - "9092:9092"
      - "9093:9093"
    volumes:
      - "kafka_data:/bitnami"
    environment:
      - KAFKA_CFG_ZOOKEEPER_CONNECT=zookeeper:2181
      - ALLOW_PLAINTEXT_LISTENER=yes
      - KAFKA_CFG_LISTENER_SECURITY_PROTOCOL_MAP=CLIENT:PLAINTEXT,EXTERNAL:PLAINTEXT
      - KAFKA_CFG_LISTENERS=CLIENT://:9092,EXTERNAL://:9093
      - KAFKA_CFG_ADVERTISED_LISTENERS=CLIENT://kafka:9092,EXTERNAL://localhost:9092
      - KAFKA_INTER_BROKER_LISTENER_NAME=CLIENT
      
    depends_on:
      - zookeeper

volumes:
  zookeeper_data:
    driver: local
  kafka_data:
    driver: local


