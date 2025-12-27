# Praktikum Data Engineering and Big Data Systems

**Kode Mata Kuliah:** CW6552021549  
**Semester:** V (Lima)  
**SKS:** 3 SKS  
**Program Studi:** Informatika  
**Fakultas:** Teknik  
**Universitas:** Universitas Muhammadiyah Makassar

---

## 📘 Deskripsi

Sistem AI yang canggih tidak akan berguna tanpa data yang berkualitas dan alur kerja yang andal. Mata kuliah ini berfokus pada siklus hidup data: penyerapan (ingestion), penyimpanan, dan pemrosesan dataset skala besar menggunakan teknologi seperti Apache Spark, Hadoop, dan layanan data berbasis cloud.

## 🎯 Capaian Pembelajaran

Setelah menyelesaikan mata kuliah ini, mahasiswa diharapkan mampu:

1. Memahami arsitektur big data dan data pipeline
2. Menguasai Apache Spark untuk distributed computing
3. Mampu membangun ETL pipeline yang robust
4. Mengimplementasikan data warehousing dan data lake
5. Menggunakan cloud services untuk big data processing

## 📚 Struktur Materi

Repositori ini mencakup materi untuk **8 pertemuan pertama** (sampai UTS):

| Pertemuan | Topik | Teknologi |
|-----------|-------|-----------|
| [01](./pertemuan-01) | **Introduction to Big Data Ecosystem** | Hadoop, Spark setup |
| [02](./pertemuan-02) | **HDFS: Hadoop Distributed File System** | HDFS operations, data ingestion |
| [03](./pertemuan-03) | **MapReduce Fundamentals** | Hadoop MapReduce, Word Count |
| [04](./pertemuan-04) | **Apache Spark: RDD & DataFrames** | Spark Core, RDD, DataFrames |
| [05](./pertemuan-05) | **Spark SQL dan Data Manipulation** | Spark SQL, query optimization |
| [06](./pertemuan-06) | **Data Streaming dengan Kafka** | Apache Kafka, Producer/Consumer |
| [07](./pertemuan-07) | **Spark Streaming** | Structured Streaming, real-time processing |
| [08](./pertemuan-08) | **UTS: Mini Project Data Pipeline** | End-to-end ETL pipeline |

## 🚀 Getting Started

### Prerequisites

**Required:**
- Java JDK 11+
- Python 3.8+
- Docker & Docker Compose (recommended)
- Git
- 8GB+ RAM
- 20GB+ disk space

**Recommended:**
- Linux/macOS (atau WSL2 untuk Windows)
- IDE: VSCode, IntelliJ, PyCharm
- Terminal emulator

### Quick Setup (Docker - Recommended)

```bash
# Clone repository
git clone https://github.com/[your-username]/data-engineering-practicum.git
cd data-engineering-practicum

# Start services
cd docker
docker-compose up -d

# Verify
docker ps

# Access services:
# - Spark UI: http://localhost:8080
# - HDFS UI: http://localhost:9870
# - Kafka UI: http://localhost:9000
```

### Manual Setup

#### Install Hadoop
```bash
# Download
wget https://dlcdn.apache.org/hadoop/common/hadoop-3.3.6/hadoop-3.3.6.tar.gz
tar -xzvf hadoop-3.3.6.tar.gz
sudo mv hadoop-3.3.6 /usr/local/hadoop

# Configure environment
export HADOOP_HOME=/usr/local/hadoop
export PATH=$PATH:$HADOOP_HOME/bin:$HADOOP_HOME/sbin
```

#### Install Spark
```bash
# Download
wget https://dlcdn.apache.org/spark/spark-3.5.0/spark-3.5.0-bin-hadoop3.tgz
tar -xzvf spark-3.5.0-bin-hadoop3.tgz
sudo mv spark-3.5.0-bin-hadoop3 /usr/local/spark

# Configure environment
export SPARK_HOME=/usr/local/spark
export PATH=$PATH:$SPARK_HOME/bin
```

#### Install Python Dependencies
```bash
pip install pyspark kafka-python pandas numpy
```

## 📖 Cara Menggunakan Repository

### Untuk Setiap Pertemuan:

1. **Baca README.md** di folder pertemuan
2. **Setup environment** sesuai instruksi
3. **Ikuti tutorial** step-by-step
4. **Kerjakan tugas** yang diberikan
5. **Submit** hasil praktikum

### Struktur Folder:

```
data-engineering-practicum/
├── README.md
├── docker/
│   └── docker-compose.yml
├── datasets/
│   └── sample-data/
├── pertemuan-01/
│   ├── README.md
│   └── [your-code]/
├── pertemuan-02/
│   └── ...
└── pertemuan-08/
    └── README.md (UTS Guidelines)
```

## 💻 Teknologi Stack

### Core Technologies
- **Apache Hadoop 3.3+**: Distributed storage & processing
- **Apache Spark 3.5+**: Unified analytics engine
- **Apache Kafka**: Event streaming platform
- **HDFS**: Distributed file system

### Programming Languages
- **Python**: PySpark, data processing
- **Scala**: Alternative untuk Spark
- **Bash**: Automation scripts
- **SQL**: Data querying

### Tools & Utilities
- **Docker**: Containerization
- **Jupyter Notebook**: Interactive development
- **Git**: Version control

## 📊 Sistem Penilaian

| Komponen | Bobot |
|----------|-------|
| Kehadiran & Partisipasi | 10% |
| Tugas Mingguan (Weekly Labs) | 30% |
| UTS (Mid-term Project) | 25% |
| UAS (Final Project & Presentation) | 35% |
| **TOTAL** | **100%** |

### Kriteria Kelulusan:
- Nilai akhir minimal: **60 (D)**
- Kehadiran minimal: **75%**
- Mengumpulkan minimal **75%** tugas
- Mengikuti UTS dan UAS

## 📝 Submission Guidelines

### Format:
```
NIM_Nama_PertemuanXX/
├── code/
│   ├── ingestion/
│   ├── processing/
│   └── README.md
├── docs/
│   └── report.pdf
└── output/
    └── results/
```

### Checklist:
- [ ] Code bisa di-run
- [ ] Documentation lengkap
- [ ] Screenshots/logs included
- [ ] Performance analysis
- [ ] Error handling implemented

## 🔧 Troubleshooting

### Common Issues:

**Out of Memory:**
```bash
# Increase Spark memory
export SPARK_DRIVER_MEMORY=4g
export SPARK_EXECUTOR_MEMORY=4g
```

**HDFS Connection Failed:**
```bash
# Check if services running
jps

# Restart HDFS
stop-dfs.sh
start-dfs.sh
```

**Kafka Not Starting:**
```bash
# Check Docker logs
docker logs kafka

# Restart services
docker-compose restart
```

## 📚 Resources

### Official Documentation:
- [Apache Hadoop](https://hadoop.apache.org/docs/stable/)
- [Apache Spark](https://spark.apache.org/docs/latest/)
- [Apache Kafka](https://kafka.apache.org/documentation/)

### Learning Resources:
- [Big Data University](https://cognitiveclass.ai/)
- [Databricks Academy](https://www.databricks.com/learn/training)
- [Confluent Kafka Tutorials](https://kafka-tutorials.confluent.io/)

### Books:
- "Hadoop: The Definitive Guide" by Tom White
- "Learning Spark" by Holden Karau
- "Designing Data-Intensive Applications" by Martin Kleppmann

## 💡 Tips Sukses

1. **Hands-on Practice**: Big Data is learned by doing
2. **Start Small**: Test locally before scaling
3. **Monitor Resources**: Watch CPU, memory, disk usage
4. **Read Logs**: Errors contain valuable debugging info
5. **Optimize**: Profile and improve performance
6. **Collaborate**: Discuss with peers, but don't plagiarize
7. **Stay Updated**: Technologies evolve rapidly

## 👥 Tim Pengajar

**Dosen Pengampu:**  
[Nama Dosen]

**Asisten Praktikum:**  
[Nama Asisten]

## 📧 Kontak & Support

- **Email:** [email dosen/asisten]
- **Office Hours:** [jadwal konsultasi]
- **Forum:** [link forum kelas]

## 📄 Lisensi

Materi ini dibuat untuk keperluan pendidikan di Universitas Muhammadiyah Makassar.

---

## 🎓 Data Engineering Principles

**Remember these principles:**

1. **Data Quality > Data Quantity**
2. **Idempotent Pipelines** - Same input = Same output
3. **Monitor Everything** - Logs, metrics, alerts
4. **Test Early, Test Often**
5. **Document for Future You**
6. **Security First** - Encrypt, authenticate, authorize
7. **Fail Fast** - Catch errors early
8. **Scale Horizontally** - Add nodes, not resources

---

## ⚠️ Catatan Penting

- **Backup data** sebelum run destructive operations
- **Test on sample** before processing full dataset
- **Plagiarism** akan mendapat sanksi
- **Deadline** adalah hard deadline
- Repository **terus diupdate** - pull regularly

---

## 🚀 Let's Build Data Pipelines!

Mulai dari [Pertemuan 01](./pertemuan-01) dan bangun foundasi Big Data Engineering Anda!

**Welcome to the World of Big Data! 📊⚡**

---

**Last Updated:** December 2024  
**Version:** 1.0  
**Maintainer:** [Nama Asisten/Dosen]
