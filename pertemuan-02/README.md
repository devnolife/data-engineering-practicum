# Pertemuan 2: HDFS - Hadoop Distributed File System

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan pertemuan ini, mahasiswa diharapkan mampu:
1. Memahami arsitektur HDFS (NameNode, DataNode)
2. Melakukan operasi dasar HDFS (upload, download, list, delete)
3. Memahami konsep replication dan fault tolerance
4. Melakukan data ingestion ke HDFS dari berbagai sumber
5. Monitoring HDFS cluster
6. Memahami block size dan replication factor

## 📚 Teori Singkat

### Apa itu HDFS?

HDFS (Hadoop Distributed File System) adalah distributed file system yang:
- Menyimpan data di multiple machines
- Highly fault-tolerant
- Optimized untuk large files
- Write-once, read-many access pattern

### Arsitektur HDFS

```
┌─────────────────────────────────────────────┐
│            NameNode (Master)                │
│  - Metadata management                      │
│  - File system namespace                    │
│  - Block mapping                            │
└─────────────────────────────────────────────┘
                │
    ┌───────────┼───────────┐
    │           │           │
┌───▼────┐  ┌──▼─────┐  ┌──▼─────┐
│DataNode│  │DataNode│  │DataNode│
│  Block │  │  Block │  │  Block │
│  Block │  │  Block │  │  Block │
│  Block │  │  Block │  │  Block │
└────────┘  └────────┘  └────────┘
```

**Komponen:**
- **NameNode**: Master server yang manage metadata
- **DataNode**: Worker nodes yang store actual data blocks
- **Secondary NameNode**: Checkpoint node (bukan backup!)

### Key Concepts

**1. Block**
- Default size: 128 MB (Hadoop 2.x+) atau 256 MB
- Large block size → Less metadata, fewer seeks
- File dipecah menjadi blocks

**2. Replication**
- Default replication factor: 3
- Blocks disimpan di multiple DataNodes
- Fault tolerance: Jika 1 node mati, data tetap available

**3. Rack Awareness**
- Blocks replicas ditempatkan di racks berbeda
- Maximize availability dan bandwidth

## 🛠️ Setup HDFS

### Start Hadoop Services

```bash
# Format NameNode (only first time)
hdfs namenode -format

# Start HDFS
start-dfs.sh

# Verify services
jps
# Output should show:
# - NameNode
# - DataNode
# - SecondaryNameNode

# Check HDFS Web UI
# http://localhost:9870
```

### Using Docker

```yaml
# docker-compose.yml
version: '3'

services:
  namenode:
    image: bde2020/hadoop-namenode:2.0.0-hadoop3.2.1-java8
    container_name: namenode
    ports:
      - 9870:9870
      - 9000:9000
    environment:
      - CLUSTER_NAME=hadoop-cluster
    env_file:
      - ./hadoop.env

  datanode:
    image: bde2020/hadoop-datanode:2.0.0-hadoop3.2.1-java8
    container_name: datanode
    depends_on:
      - namenode
    environment:
      - SERVICE_PRECONDITION=namenode:9870
    env_file:
      - ./hadoop.env
```

```bash
# Start
docker-compose up -d

# Check
docker ps
```

## 📝 Praktikum

### Langkah 1: HDFS Basic Commands

```bash
# Create directory
hdfs dfs -mkdir /user
hdfs dfs -mkdir /user/hadoop
hdfs dfs -mkdir /data

# List directory
hdfs dfs -ls /
hdfs dfs -ls /user

# Upload file to HDFS
echo "Hello HDFS!" > local_file.txt
hdfs dfs -put local_file.txt /user/hadoop/

# List files
hdfs dfs -ls /user/hadoop

# View file content
hdfs dfs -cat /user/hadoop/local_file.txt

# Download file from HDFS
hdfs dfs -get /user/hadoop/local_file.txt downloaded_file.txt

# Copy within HDFS
hdfs dfs -cp /user/hadoop/local_file.txt /data/

# Move within HDFS
hdfs dfs -mv /data/local_file.txt /data/renamed_file.txt

# Delete file
hdfs dfs -rm /data/renamed_file.txt

# Delete directory (recursive)
hdfs dfs -rm -r /data
```

### Langkah 2: Working with Large Files

```bash
# Generate large file (100 MB)
dd if=/dev/zero of=large_file.bin bs=1M count=100

# Upload to HDFS
hdfs dfs -put large_file.bin /user/hadoop/

# Check file size
hdfs dfs -du -h /user/hadoop/large_file.bin

# Check file blocks
hdfs fsck /user/hadoop/large_file.bin -files -blocks -locations

# Example output:
# /user/hadoop/large_file.bin:
# Total size:    104857600 B
# Total blocks:  1 (avg. block size 104857600 B)
# Minimally replicated blocks: 1 (100.0 %)
```

### Langkah 3: File Replication

```bash
# Check replication factor
hdfs dfs -stat "%r" /user/hadoop/large_file.bin

# Change replication factor
hdfs dfs -setrep 2 /user/hadoop/large_file.bin

# Verify
hdfs fsck /user/hadoop/large_file.bin -files -blocks -racks

# Set replication for entire directory
hdfs dfs -setrep -R 3 /user/hadoop/
```

### Langkah 4: Bulk Data Ingestion

**Python script: `ingest_data.py`**
```python
import subprocess
import os

def upload_to_hdfs(local_path, hdfs_path):
    """Upload file atau directory ke HDFS"""
    try:
        cmd = f"hdfs dfs -put {local_path} {hdfs_path}"
        result = subprocess.run(cmd, shell=True, capture_output=True, text=True)
        
        if result.returncode == 0:
            print(f"✓ Successfully uploaded {local_path} to {hdfs_path}")
        else:
            print(f"✗ Error: {result.stderr}")
            
    except Exception as e:
        print(f"✗ Exception: {str(e)}")

def create_hdfs_directory(hdfs_path):
    """Create directory di HDFS"""
    cmd = f"hdfs dfs -mkdir -p {hdfs_path}"
    subprocess.run(cmd, shell=True)
    print(f"✓ Created directory: {hdfs_path}")

# Example usage
if __name__ == "__main__":
    # Create HDFS directories
    create_hdfs_directory("/data/raw")
    create_hdfs_directory("/data/processed")
    
    # Upload files
    upload_to_hdfs("dataset.csv", "/data/raw/")
    upload_to_hdfs("logs/", "/data/raw/logs/")
    
    # List uploaded files
    subprocess.run("hdfs dfs -ls -R /data/", shell=True)
```

### Langkah 5: Monitor HDFS Cluster

```bash
# HDFS report (cluster summary)
hdfs dfsadmin -report

# Output includes:
# - Configured Capacity
# - DFS Used
# - DFS Remaining
# - Live DataNodes
# - Dead DataNodes

# Check cluster health
hdfs dfsadmin -safemode get

# File system check
hdfs fsck / -files -blocks -locations

# Get DataNode info
hdfs dfsadmin -printTopology

# Disk usage
hdfs dfs -df -h
hdfs dfs -du -h /user/hadoop
```

### Langkah 6: Data Ingestion Pipeline

**Script: `csv_ingestion.py`**
```python
from pyspark.sql import SparkSession
import sys

def ingest_csv_to_hdfs(local_csv, hdfs_path):
    """
    Load CSV dari local filesystem dan save ke HDFS dalam Parquet format
    """
    # Create Spark session
    spark = SparkSession.builder \
        .appName("CSVIngestion") \
        .getOrCreate()
    
    try:
        # Read CSV
        print(f"Reading {local_csv}...")
        df = spark.read.csv(local_csv, header=True, inferSchema=True)
        
        # Show sample
        print("\n=== Sample Data ===")
        df.show(5)
        
        print(f"\n=== Schema ===")
        df.printSchema()
        
        print(f"\n=== Statistics ===")
        print(f"Total rows: {df.count()}")
        print(f"Total columns: {len(df.columns)}")
        
        # Save to HDFS as Parquet
        output_path = f"{hdfs_path}/data.parquet"
        print(f"\nSaving to HDFS: {output_path}")
        
        df.write.parquet(output_path, mode="overwrite")
        
        print("✓ Data successfully ingested to HDFS")
        
        # Verify
        print(f"\n=== Verifying HDFS ===")
        df_verify = spark.read.parquet(output_path)
        print(f"Rows in HDFS: {df_verify.count()}")
        
    except Exception as e:
        print(f"✗ Error: {str(e)}")
        sys.exit(1)
    finally:
        spark.stop()

if __name__ == "__main__":
    if len(sys.argv) != 3:
        print("Usage: spark-submit csv_ingestion.py <local_csv> <hdfs_path>")
        sys.exit(1)
    
    local_csv = sys.argv[1]
    hdfs_path = sys.argv[2]
    
    ingest_csv_to_hdfs(local_csv, hdfs_path)
```

**Run:**
```bash
spark-submit csv_ingestion.py sales_data.csv hdfs://localhost:9000/data/sales
```

### Langkah 7: Batch File Upload

**Script: `batch_upload.sh`**
```bash
#!/bin/bash

# Batch upload multiple files to HDFS

SOURCE_DIR="./local_data"
HDFS_DIR="/data/batch_upload"

# Create HDFS directory
hdfs dfs -mkdir -p $HDFS_DIR

# Upload all CSV files
for file in $SOURCE_DIR/*.csv; do
    filename=$(basename "$file")
    echo "Uploading $filename..."
    hdfs dfs -put "$file" "$HDFS_DIR/"
    
    if [ $? -eq 0 ]; then
        echo "✓ $filename uploaded successfully"
    else
        echo "✗ Failed to upload $filename"
    fi
done

# Show uploaded files
echo -e "\n=== Files in HDFS ==="
hdfs dfs -ls $HDFS_DIR

# Show total size
echo -e "\n=== Total Size ==="
hdfs dfs -du -s -h $HDFS_DIR
```

## 💪 Tugas Praktikum

### Tugas 1: HDFS Basic Operations (20 poin)

Buat script bash yang melakukan:
1. Create folder structure di HDFS:
   ```
   /project
   ├── raw
   ├── processed
   └── archive
   ```
2. Upload 5 file berbeda ke `/project/raw`
3. List semua files dengan size
4. Copy 2 files ke `/project/processed`
5. Delete 1 file dari `/project/raw`
6. Generate report summary (total files, total size)

### Tugas 2: Replication Management (20 poin)

1. Upload file berukuran > 200 MB ke HDFS
2. Check default replication factor
3. Ubah replication factor menjadi 5
4. Monitoring perubahan dengan `hdfs fsck`
5. Analisis:
   - Berapa block yang terbentuk?
   - Bagaimana block distribution di DataNodes?
   - Berapa total storage yang digunakan?
6. Screenshot HDFS Web UI yang menunjukkan block info

### Tugas 3: Data Ingestion Pipeline (35 poin)

Download atau buat dataset CSV (min. 10,000 rows):

Buat Python script yang:
1. Read CSV dari local filesystem
2. Perform basic validation (check missing values, data types)
3. Clean data (handle missing values, remove duplicates)
4. Save hasil ke HDFS dalam 3 format:
   - CSV
   - JSON
   - Parquet
5. Compare file sizes di HDFS
6. Benchmark read performance untuk ketiga format
7. Buat laporan:
   - Execution time
   - File size comparison
   - Read performance
   - Recommendation: Format mana yang terbaik?

### Tugas 4: HDFS Monitoring & Health Check (25 poin)

Buat monitoring script yang:
1. Check HDFS cluster status (`dfsadmin -report`)
2. Parse output untuk extract:
   - Total capacity
   - Used space
   - Available space
   - Number of live/dead DataNodes
3. Check for corrupt blocks (`hdfs fsck`)
4. Generate HTML report dengan visualisasi
5. Implement alert system (jika used space > 80%)

**Bonus (+10):** Automate monitoring dengan cron job

## 📤 Cara Mengumpulkan

1. Source code semua scripts (.sh, .py)
2. Screenshot output execution
3. Screenshot HDFS Web UI
4. Laporan PDF dengan analisis
5. Compress: `NIM_Nama_Pertemuan02.zip`
6. Upload ke LMS

## ✅ Kriteria Penilaian

| Aspek | Bobot |
|-------|-------|
| Tugas 1: Basic Operations | 20% |
| Tugas 2: Replication | 20% |
| Tugas 3: Ingestion Pipeline | 35% |
| Tugas 4: Monitoring | 25% |
| Documentation | 15% |
| Code quality | 10% |

## 📚 Referensi

1. [HDFS Architecture Guide](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)
2. [HDFS Commands Guide](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HDFSCommands.html)
3. [HDFS User Guide](https://hadoop.apache.org/docs/stable/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html)

## 💡 Tips

- Default block size = 128 MB, untuk file kecil bisa inefficient
- Replication factor 3 adalah standard untuk production
- Gunakan **Parquet** untuk analytics workload (columnar format)
- HDFS bagus untuk write-once, read-many pattern
- Monitor disk usage regularly
- Use `-copyFromLocal` instead of `-put` untuk clarity

---

**Happy Data Ingesting! 🚀📁**
