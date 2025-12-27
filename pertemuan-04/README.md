# Pertemuan 4: Apache Spark - RDD & DataFrames

## 🎯 Tujuan Pembelajaran

1. Memahami konsep RDD (Resilient Distributed Dataset)
2. Melakukan transformasi dan action pada RDD
3. Bekerja dengan Spark DataFrames
4. Memahami lazy evaluation dan DAG execution
5. Optimasi Spark jobs

## 📚 Teori Singkat

### RDD (Resilient Distributed Dataset)

**Karakteristik:**
- Immutable
- Distributed across cluster
- Fault-tolerant (lineage)
- Lazy evaluation

**Operations:**
- **Transformations**: map, filter, flatMap, union, join (lazy)
- **Actions**: collect, count, save, reduce (trigger execution)

### DataFrame

- Structured data dengan schema
- Optimized execution (Catalyst optimizer)
- Support SQL queries
- Better performance than RDD

## 📝 Praktikum

### RDD Operations

```python
from pyspark import SparkContext

sc = SparkContext("local", "RDDExample")

# Create RDD
data = [1, 2, 3, 4, 5]
rdd = sc.parallelize(data)

# Transformations
squared = rdd.map(lambda x: x * x)
filtered = squared.filter(lambda x: x > 10)

# Actions
print(filtered.collect())  # [16, 25]
print(filtered.count())     # 2

sc.stop()
```

### DataFrame Operations

```python
from pyspark.sql import SparkSession

spark = SparkSession.builder.appName("DataFrameExample").getOrCreate()

# Create DataFrame
data = [("Alice", 25), ("Bob", 30), ("Charlie", 35)]
df = spark.createDataFrame(data, ["Name", "Age"])

df.show()
df.filter(df.Age > 28).show()
df.groupBy("Age").count().show()

spark.stop()
```

## 💪 Tugas Praktikum

### Tugas 1: RDD Transformations (25%)
Implementasikan:
- map, filter, flatMap
- reduceByKey, groupByKey
- join operations

### Tugas 2: DataFrame Analysis (30%)
Load CSV dataset:
- Data cleaning
- Aggregations
- Joins
- Window functions

### Tugas 3: Performance Comparison (25%)
Benchmark: RDD vs DataFrame untuk sama task

### Tugas 4: Real-World Application (20%)
Process log files dengan Spark

## 📚 Referensi

1. [Spark RDD Programming Guide](https://spark.apache.org/docs/latest/rdd-programming-guide.html)
2. [Spark SQL Guide](https://spark.apache.org/docs/latest/sql-programming-guide.html)

---

**Happy Sparking! ⚡**
