# Pertemuan 7: Spark Streaming untuk Real-time Processing

## 🎯 Tujuan Pembelajaran

1. Memahami Spark Structured Streaming
2. Integrate Spark dengan Kafka
3. Implement real-time analytics
4. Handle late data dan watermarks
5. Stream processing pipeline end-to-end

## 📚 Teori Singkat

### Spark Structured Streaming

**Concepts:**
- Micro-batch processing
- Continuous processing
- Stateful operations
- Window operations
- Watermarks for late data

### Integration
- Kafka source/sink
- File source/sink
- Socket source
- Console sink

## 📝 Praktikum

### Basic Streaming

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import *

spark = SparkSession.builder \
    .appName("Streaming") \
    .getOrCreate()

# Read stream from Kafka
df = spark \
    .readStream \
    .format("kafka") \
    .option("kafka.bootstrap.servers", "localhost:9092") \
    .option("subscribe", "my-topic") \
    .load()

# Process
result = df.selectExpr("CAST(value AS STRING)") \
    .select(from_json(col("value"), schema).alias("data")) \
    .select("data.*") \
    .groupBy("category") \
    .count()

# Write stream
query = result \
    .writeStream \
    .outputMode("complete") \
    .format("console") \
    .start()

query.awaitTermination()
```

### Window Operations

```python
from pyspark.sql.functions import window

windowed = df \
    .groupBy(
        window("timestamp", "1 minute", "30 seconds"),
        "event_type"
    ) \
    .count()
```

## 💪 Tugas Praktikum

### Tugas 1: Kafka + Spark Integration (25%)
Read from Kafka, process, write to HDFS

### Tugas 2: Real-time Analytics (30%)
Build dashboard:
- Moving averages
- Trend detection
- Anomaly detection

### Tugas 3: Stateful Processing (25%)
Implement:
- Session windows
- Join streams
- Deduplication

### Tugas 4: End-to-End Pipeline (20%)
Complete streaming pipeline:
Kafka → Spark → Database/Dashboard

## 📚 Referensi

[Structured Streaming Guide](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html)

---
**Stream in Real-Time! ⚡🌊**
