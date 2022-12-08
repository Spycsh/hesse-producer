# hesse-producer

The code serves as a docker image and it enables producing messages into multiple Kafka partitions in one topic. It will produce to the partition based on hashing the key (APP_JSON_PATH) % KAFKA_PARTITIONS_NUM. The KAFKA_PARTITIONS_NUM is the number of Kafka partitions specified by users.

## How to use

The code serves hesse as a temporal-graph-producer container. Here is one example with three steps to produce edges to 2 Kafka partitions and let hesse route to the storage layer.

1. docker-compose.yml

```
  hesse-graph-producer:
    image: spycsh/hesse-producer:latest # pull this image
    depends_on:
    - kafka
    links:
    - kafka:kafka
    environment:
      APP_PATH: /mnt/datasets/graph/email_edges_undirected.txt
      APP_KAFKA_HOST: kafka:9092
      APP_KAFKA_TOPIC: temporal-graph
      APP_DELAY_SECONDS: 0
      APP_LOOP: 'false'
      APP_JSON_PATH: src_id
      KAFKA_PARTITIONS_NUM: 2   # specify the partition number
    volumes:
    - ./datasets/graph/email_edges_undirected.txt:/mnt/datasets/graph/email_edges_undirected.txt
```

2. module.yaml

```
---
kind: io.statefun.kafka.v1/ingress
spec:
  id: hesse.io/temporal-graph
  address: kafka:9092
  consumerGroupId: hesse
  startupPosition:
    type: specific-offsets
    offsets:
      - temporal-graph/0: 0
      - temporal-graph/1: 0
  topics:
  - topic: temporal-graph
    valueType: hesse.types/temporal_edge
    targets:
    - hesse.storage/vertex-storage
```

3. Change the `flink-conf.yaml` parallelism to 2

```
taskmanager.numberOfTaskSlots: 2
parallelism.default: 2
```

This is still experimental and can be correctly runned after testing. However, the performance has not been thoroughly tested.

## Build

```
# push the recent changes
git commit -a -m "edit"
git push
# pull the original image and edit
docker pull spycsh/graph-producer
docker exec -it <container id> /bin/sh
rm -f main.py
wget https://raw.githubusercontent.com/Spycsh/hesse-producer/main/main.py
exit
docker commit -m="specify partition number" -a="spycsh" <container id> spycsh/hesse-producer
docker login
docker push spycsh/hesse-producer
```