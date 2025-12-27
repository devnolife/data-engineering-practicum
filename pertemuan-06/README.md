# Pertemuan 6: Data Streaming dengan Apache Kafka

## 🎯 Tujuan Pembelajaran

1. Memahami konsep streaming data
2. Setup dan konfigurasi Apache Kafka
3. Implementasi Producer dan Consumer
4. Kafka Topics dan Partitions
5. Real-time data ingestion

## 📚 Teori Singkat

### Apache Kafka

**Komponen:**
- **Producer**: Publish messages ke topic
- **Consumer**: Subscribe dan consume messages
- **Broker**: Kafka server
- **Topic**: Category untuk messages
- **Partition**: Parallel processing unit

### Use Cases
- Real-time analytics
- Log aggregation
- Event sourcing
- Stream processing

## 📝 Praktikum

### Setup Kafka (Docker)

```yaml
# docker-compose.yml
version: '3'
services:
  zookeeper:
    image: confluentinc/cp-zookeeper:latest
    environment:
      ZOOKEEPER_CLIENT_PORT: 2181
      
  kafka:
    image: confluentinc/cp-kafka:latest
    depends_on:
      - zookeeper
    ports:
      - "9092:9092"
    environment:
      KAFKA_ZOOKEEPER_CONNECT: zookeeper:2181
      KAFKA_ADVERTISED_LISTENERS: PLAINTEXT://localhost:9092
```

### Producer

```python
from kafka import KafkaProducer
import json

producer = KafkaProducer(
    bootstrap_servers=['localhost:9092'],
    value_serializer=lambda v: json.dumps(v).encode('utf-8')
)

# Send message
producer.send('my-topic', {'event': 'purchase', 'amount': 100})
producer.flush()
```

### Consumer

```python
from kafka import KafkaConsumer
import json

consumer = KafkaConsumer(
    'my-topic',
    bootstrap_servers=['localhost:9092'],
    value_deserializer=lambda m: json.loads(m.decode('utf-8'))
)

for message in consumer:
    print(message.value)
```

## 💪 Tugas Praktikum

### Tugas 1: Kafka Setup (20%)
Install dan configure Kafka cluster

### Tugas 2: Producer Application (25%)
Stream data dari:
- CSV file
- API
- IoT sensor simulation

### Tugas 3: Consumer Application (25%)
Process streaming data:
- Real-time aggregation
- Filtering
- Transform

### Tugas 4: Multi-Topic Pipeline (30%)
Build streaming pipeline dengan multiple topics

## 📚 Referensi

[Apache Kafka Documentation](https://kafka.apache.org/documentation/)

---
**Stream On! 🌊**
