Here's a concise and clear `kafka-docker-setup.md` file explaining your ZooKeeper and Kafka container setup:

---

````markdown
# Kafka & ZooKeeper Setup Using Docker

This guide explains how to run **ZooKeeper** and **Kafka** containers using Docker, using Confluent's official Docker images.

---

## 🐘 Step 1: Start ZooKeeper Container

Kafka requires ZooKeeper to manage its cluster. Start ZooKeeper with the following command:

```bash
docker run -d \
  --name zookeeper \
  -p 2181:2181 \
  -e ZOOKEEPER_CLIENT_PORT=2181 \
  -e ZOOKEEPER_TICK_TIME=2000 \
  confluentinc/cp-zookeeper:7.5.0
````

### 🔹 Description:

* `--name zookeeper`: Names the container "zookeeper"
* `-p 2181:2181`: Maps port 2181 (ZooKeeper's default) to host
* `ZOOKEEPER_CLIENT_PORT`: Port on which ZooKeeper listens
* `ZOOKEEPER_TICK_TIME`: Time unit used by ZooKeeper for heartbeats and timeouts

---

## ⚙️ Step 2: Start Kafka Container

Once ZooKeeper is running, start the Kafka broker:

```bash
docker run -d \
  --name kafka \
  -p 9092:9092 \
  -e KAFKA_BROKER_ID=1 \
  -e KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181 \
  -e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://localhost:9092 \
  -e KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1 \
  --link zookeeper \
  confluentinc/cp-kafka:7.5.0
```

### 🔹 Description:

* `--name kafka`: Names the container "kafka"
* `-p 9092:9092`: Maps Kafka port to host
* `KAFKA_BROKER_ID`: Unique ID for the Kafka broker
* `KAFKA_ZOOKEEPER_CONNECT`: Points to ZooKeeper (`zookeeper` is resolvable via `--link`)
* `KAFKA_ADVERTISED_LISTENERS`: Tells Kafka how to advertise itself to clients
* `KAFKA_OFFSETS_TOPIC_REPLICATION_FACTOR=1`: Required for single-node setup
* `--link zookeeper`: Links the ZooKeeper container for hostname resolution
* `confluentinc/cp-kafka:7.5.0`: **Correct Kafka image** (not the ZooKeeper one)

---

## ⚠️ Note:

You had a typo in your Kafka container command:

* `KAFKA_ZOOKEEPER_CONNECT:2181` should be `KAFKA_ZOOKEEPER_CONNECT=zookeeper:2181`
* `KAFKA_ADVERTISED_KISTENERS` should be `KAFKA_ADVERTISED_LISTENERS`
* You used the ZooKeeper image (`cp-zookeeper`) for Kafka — it should be `cp-kafka`

---

## ✅ Final Result

After both containers are running:

* ZooKeeper is accessible at `localhost:2181`
* Kafka is accessible at `localhost:9092`

You can now produce and consume Kafka messages locally.

---
