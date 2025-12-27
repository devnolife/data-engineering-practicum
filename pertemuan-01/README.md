# Pertemuan 1: Introduction to Big Data Ecosystem

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan pertemuan ini, mahasiswa diharapkan mampu:
1. Memahami konsep Big Data dan karakteristiknya (5V)
2. Mengenal arsitektur Big Data ecosystem
3. Menginstall dan mengkonfigurasi Hadoop
4. Menginstall dan mengkonfigurasi Apache Spark
5. Menjalankan aplikasi Spark sederhana
6. Memahami perbedaan batch processing vs stream processing

## 📚 Teori Singkat

### Apa itu Big Data?

Big Data adalah kumpulan data yang sangat besar, kompleks, dan berkembang cepat sehingga sulit diproses dengan tools tradisional.

**5V of Big Data:**
1. **Volume**: Jumlah data yang sangat besar (TB, PB, EB)
2. **Velocity**: Kecepatan data dihasilkan dan diproses
3. **Variety**: Beragam format (structured, semi-structured, unstructured)
4. **Veracity**: Kualitas dan akurasi data
5. **Value**: Nilai bisnis yang dapat diekstrak dari data

### Big Data Ecosystem

```
┌─────────────────────────────────────────┐
│         Data Sources                    │
│  (IoT, Social Media, Logs, Databases)   │
└─────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Data Ingestion                  │
│    (Kafka, Flume, Sqoop, NiFi)         │
└─────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Data Storage                    │
│    (HDFS, S3, NoSQL, Data Lake)        │
└─────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Data Processing                 │
│    (Hadoop MapReduce, Spark)           │
└─────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Data Analysis                   │
│    (Spark SQL, Hive, Presto)           │
└─────────────────────────────────────────┘
              │
              ▼
┌─────────────────────────────────────────┐
│         Visualization                   │
│    (Tableau, PowerBI, Grafana)         │
└─────────────────────────────────────────┘
```

### Apache Hadoop

**Komponen Utama:**
- **HDFS (Hadoop Distributed File System)**: Distributed storage
- **YARN (Yet Another Resource Negotiator)**: Resource management
- **MapReduce**: Programming model untuk distributed processing

### Apache Spark

**Keunggulan:**
- 100x lebih cepat dari Hadoop MapReduce (in-memory processing)
- Support multiple languages (Python, Scala, Java, R)
- Unified engine (batch, streaming, ML, graph processing)

**Komponen:**
- **Spark Core**: Basic functionality
- **Spark SQL**: Structured data processing
- **Spark Streaming**: Real-time data processing
- **MLlib**: Machine learning library
- **GraphX**: Graph processing

## 🛠️ Setup Environment

### Option 1: Install di Local (Linux/Mac)

#### Install Java
```bash
# Ubuntu/Debian
sudo apt update
sudo apt install openjdk-11-jdk

# Verify
java -version
```

#### Install Hadoop
```bash
# Download Hadoop
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz

# Extract
tar -xzvf hadoop-3.3.6.tar.gz
sudo mv hadoop-3.3.6 /usr/local/hadoop

# Set environment variables
echo 'export HADOOP_HOME=/usr/local/hadoop' >> ~/.bashrc
echo 'export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin' >> ~/.bashrc
source ~/.bashrc

# Verify
hadoop version
```

#### Install Spark
```bash
# Download Spark
wget https://dlcdn.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz

# Extract
tar -xzvf spark-3.5.0-bin-hadoop3.tgz
sudo mv spark-3.5.0-bin-hadoop3 /usr/local/spark

# Set environment variables
echo 'export SPARK_HOME=/usr/local/spark' >> ~/.bashrc
echo 'export PATH=$PATH:$SPARK_HOME/bin:$SPARK_HOME/sbin' >> ~/.bashrc
source ~/.bashrc

# Verify
spark-shell --version
```

### Option 2: Menggunakan Docker (Recommended)

#### Install Docker
```bash
# Ubuntu
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh

# Verify
docker --version
docker-compose --version
```

#### Create Docker Compose File
```yaml
# docker-compose.yml
version: '3'

services:
  spark-master:
    image: bitnami/spark:3.5.0
    container_name: spark-master
    environment:
      - SPARK_MODE=master
      - SPARK_RPC_AUTHENTICATION_ENABLED=no
      - SPARK_RPC_ENCRYPTION_ENABLED=no
    ports:
      - "8080:8080"
      - "7077:7077"
    volumes:
      - ./workspace:/workspace

  spark-worker:
    image: bitnami/spark:3.5.0
    container_name: spark-worker
    environment:
      - SPARK_MODE=worker
      - SPARK_MASTER_URL=spark://spark-master:7077
      - SPARK_WORKER_MEMORY=2G
      - SPARK_WORKER_CORES=2
    depends_on:
      - spark-master
```

#### Start Services
```bash
# Start containers
docker-compose up -d

# Check status
docker ps

# Access Spark UI
# Open browser: http://localhost:8080
```

### Option 3: Google Colab (Untuk Belajar)

```python
# Install PySpark di Google Colab
!pip install pyspark

# Import library
from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder \
    .appName("BigDataPracticum") \
    .getOrCreate()

print(spark.version)
```

## 📝 Praktikum

### Langkah 1: Verifikasi Instalasi

```bash
# Check Hadoop
hadoop version

# Check Spark
spark-shell --version
pyspark --version
```

### Langkah 2: Spark Shell - Interactive Mode

```bash
# Start Spark Shell (Scala)
spark-shell

# atau PySpark (Python)
pyspark
```

**Di PySpark shell:**
```python
# Create simple RDD
data = [1, 2, 3, 4, 5]
rdd = sc.parallelize(data)

# Basic operations
print(rdd.count())  # 5
print(rdd.first())  # 1
print(rdd.take(3))  # [1, 2, 3]

# Transformations
squared = rdd.map(lambda x: x * x)
print(squared.collect())  # [1, 4, 9, 16, 25]

# Exit
exit()
```

### Langkah 3: First Spark Application (Python)

**File: `word_count.py`**
```python
from pyspark.sql import SparkSession

# Create Spark session
spark = SparkSession.builder \
    .appName("WordCount") \
    .master("local[*]") \
    .getOrCreate()

# Sample text
text = """
Big Data is everywhere
Data Engineering is important
Apache Spark is fast
Hadoop is distributed
"""

# Create RDD
lines = spark.sparkContext.parallelize(text.strip().split('\n'))

# Word count
words = lines.flatMap(lambda line: line.split(" "))
word_pairs = words.map(lambda word: (word.lower(), 1))
word_counts = word_pairs.reduceByKey(lambda a, b: a + b)

# Collect and print results
results = word_counts.collect()
for word, count in sorted(results, key=lambda x: x[1], reverse=True):
    print(f"{word}: {count}")

# Stop Spark
spark.stop()
```

**Jalankan:**
```bash
spark-submit word_count.py
```

### Langkah 4: Spark DataFrame API

**File: `dataframe_intro.py`**
```python
from pyspark.sql import SparkSession
from pyspark.sql.functions import col, avg, count

# Create Spark session
spark = SparkSession.builder \
    .appName("DataFrameIntro") \
    .getOrCreate()

# Create sample data
data = [
    ("Alice", 25, "Engineering", 75000),
    ("Bob", 30, "Sales", 60000),
    ("Charlie", 35, "Engineering", 85000),
    ("David", 28, "Marketing", 55000),
    ("Eve", 32, "Sales", 70000),
    ("Frank", 29, "Engineering", 80000)
]

columns = ["Name", "Age", "Department", "Salary"]

# Create DataFrame
df = spark.createDataFrame(data, columns)

# Show data
print("=== Original Data ===")
df.show()

# Schema
print("\n=== Schema ===")
df.printSchema()

# Basic statistics
print("\n=== Statistics ===")
df.describe().show()

# Filtering
print("\n=== Engineering Department ===")
df.filter(col("Department") == "Engineering").show()

# Aggregation
print("\n=== Average Salary by Department ===")
df.groupBy("Department") \
  .agg(avg("Salary").alias("AvgSalary"), 
       count("Name").alias("Count")) \
  .show()

# Sorting
print("\n=== Top 3 Highest Salaries ===")
df.orderBy(col("Salary").desc()).limit(3).show()

# Stop Spark
spark.stop()
```

### Langkah 5: Reading and Writing Files

**File: `file_operations.py`**
```python
from pyspark.sql import SparkSession

spark = SparkSession.builder \
    .appName("FileOperations") \
    .getOrCreate()

# Create sample data
data = [
    ("Product A", 100, 25.5),
    ("Product B", 150, 30.0),
    ("Product C", 200, 15.75),
    ("Product D", 75, 45.25),
    ("Product E", 300, 20.0)
]

df = spark.createDataFrame(data, ["Product", "Quantity", "Price"])

# Write to CSV
df.write.csv("output/products.csv", header=True, mode="overwrite")
print("Data written to CSV")

# Write to JSON
df.write.json("output/products.json", mode="overwrite")
print("Data written to JSON")

# Write to Parquet (columnar format - recommended for big data)
df.write.parquet("output/products.parquet", mode="overwrite")
print("Data written to Parquet")

# Read back
print("\n=== Reading from Parquet ===")
df_parquet = spark.read.parquet("output/products.parquet")
df_parquet.show()

# Stop Spark
spark.stop()
```

### Langkah 6: Monitoring Spark Jobs

```bash
# Start Spark application
spark-submit your_script.py

# Open Spark UI in browser
# http://localhost:4040

# View:
# - Jobs
# - Stages
# - Storage
# - Environment
# - Executors
```

## 💪 Tugas Praktikum

### Tugas 1: Environment Setup (20 poin)

1. Install Hadoop dan Spark (pilih salah satu: local, Docker, atau Colab)
2. Verifikasi instalasi dengan screenshot:
   - `hadoop version`
   - `spark-shell --version`
   - Spark UI (http://localhost:8080)
3. Dokumentasikan langkah instalasi yang Anda lakukan
4. Identifikasi masalah yang dihadapi (jika ada) dan cara mengatasinya

### Tugas 2: Spark RDD Operations (25 poin)

Buat program Python yang melakukan:
1. Create RDD dari list angka 1-100
2. Operasi transformasi:
   - Filter angka genap
   - Map: kuadratkan setiap angka
   - Filter angka > 500
3. Operasi action:
   - Count jumlah elemen
   - Sum total nilai
   - Take 10 nilai tertinggi
4. Hitung waktu eksekusi untuk setiap operasi

### Tugas 3: DataFrame Analysis (30 poin)

Download dataset CSV (contoh: Titanic, Sales, atau dataset bebas dari Kaggle).

Buat Spark application yang:
1. Load dataset ke DataFrame
2. Tampilkan schema dan 10 baris pertama
3. Lakukan data cleaning:
   - Handle missing values
   - Remove duplicates
4. Analisis deskriptif:
   - Count total rows
   - Summary statistics
   - Group by dan aggregation (minimal 2 kolom)
5. Filter dan sort data berdasarkan kondisi tertentu
6. Save hasil ke Parquet format

### Tugas 4: Comparison Study (25 poin)

Buat laporan perbandingan:

**Traditional Processing vs Big Data Processing**

| Aspect | Traditional (Pandas/SQL) | Big Data (Spark) |
|--------|-------------------------|------------------|
| Data Size Limit | | |
| Processing Speed | | |
| Scalability | | |
| Memory Usage | | |
| Use Cases | | |

Sertakan:
1. Contoh kode untuk operasi yang sama di Pandas dan Spark
2. Benchmark sederhana (waktu eksekusi untuk dataset kecil vs besar)
3. Kapan sebaiknya menggunakan Spark vs traditional tools?
4. Kesimpulan dan rekomendasi

## 📤 Cara Mengumpulkan

1. **Tugas 1**: Screenshot + dokumentasi dalam PDF
2. **Tugas 2-3**: Source code (.py) + screenshot output + penjelasan
3. **Tugas 4**: Laporan PDF dengan grafik perbandingan
4. Compress semua file: `NIM_Nama_Pertemuan01.zip`
5. Upload ke LMS

## ✅ Kriteria Penilaian

| Aspek | Bobot |
|-------|-------|
| Tugas 1: Setup Environment | 20% |
| Tugas 2: RDD Operations | 25% |
| Tugas 3: DataFrame Analysis | 30% |
| Tugas 4: Comparison Study | 25% |
| Dokumentasi & penjelasan | 15% |
| Code quality | 10% |

## 📚 Referensi

1. [Apache Spark Documentation](https://spark.apache.org/docs/latest/)
2. [Hadoop Documentation](https://hadoop.apache.org/docs/stable/)
3. [PySpark Tutorial](https://spark.apache.org/docs/latest/api/python/)
4. [Spark by Examples](https://sparkbyexamples.com/)
5. [Big Data Architecture](https://www.databricks.com/glossary/big-data-architecture)

## 💡 Tips

- **Docker** adalah cara termudah untuk setup Hadoop/Spark
- **Google Colab** bagus untuk belajar konsep tanpa install
- **PySpark** lebih mudah dipelajari daripada Scala untuk pemula
- Selalu gunakan `.show()` untuk preview DataFrame
- Monitor Spark UI untuk understand job execution
- Gunakan **Parquet** untuk storage format (lebih efisien)

## 🆘 Troubleshooting

### Out of Memory Error
```python
# Reduce memory usage
spark = SparkSession.builder \
    .config("spark.driver.memory", "4g") \
    .config("spark.executor.memory", "2g") \
    .getOrCreate()
```

### Slow Performance
```python
# Increase parallelism
df.repartition(8)  # 8 partitions
```

### Port Already in Use
```bash
# Change default port
spark-shell --master local[*] --driver-port 4041
```

---

**Selamat Belajar Big Data! 🚀📊**

Jika ada pertanyaan, silakan diskusikan di forum kelas atau hubungi asisten praktikum.
