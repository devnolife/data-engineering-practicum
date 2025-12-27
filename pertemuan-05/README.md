# Pertemuan 5: Spark SQL dan Data Manipulation

## 🎯 Tujuan Pembelajaran

1. Menguasai Spark SQL untuk query data
2. Melakukan data manipulation dengan DataFrame API
3. Mengoptimalkan query performance
4. Bekerja dengan berbagai data sources

## 📚 Teori Singkat

### Spark SQL
- SQL queries pada distributed data
- Catalyst optimizer
- Multiple format support

### Optimization
- Partitioning
- Caching
- Broadcast joins
- Predicate pushdown

## 📝 Praktikum

```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, sum, desc

spark = SparkSession.builder.appName("SparkSQL").getOrCreate()

# SQL
df.createOrReplaceTempView("sales")
spark.sql("SELECT product, SUM(quantity) FROM sales GROUP BY product").show()

# DataFrame API
df.groupBy("product").agg(sum("quantity")).orderBy(desc("quantity")).show()
```

## 💪 Tugas Praktikum

### Tugas 1: SQL Queries (30%)
Multi-table joins, subqueries, window functions

### Tugas 2: ETL Pipeline (35%)
Extract, Transform, Load dengan Spark SQL

### Tugas 3: Query Optimization (20%)
Execution plan analysis, caching, partitioning

### Tugas 4: Integration (15%)
Connect ke databases dan cloud storage

## 📚 Referensi

[Spark SQL Documentation](https://spark.apache.org/docs/latest/sql-programming-guide.html)

---
**Master Spark SQL! 🔍**
