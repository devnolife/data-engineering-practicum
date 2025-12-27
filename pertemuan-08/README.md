# Pertemuan 8: UTS - Mini Project Data Pipeline

## 🎯 Tujuan UTS

Evaluasi pemahaman mahasiswa terhadap:
1. Complete data pipeline (ingestion → processing → storage)
2. Big Data technologies (Hadoop, Spark, Kafka)
3. Data engineering best practices
4. Documentation dan deployment

## 📋 Project Requirements

### Pilihan Project

**Option 1: Batch Processing Pipeline**
- Data ingestion ke HDFS
- Processing dengan Spark
- Aggregations dan transformations
- Output ke data warehouse

**Option 2: Streaming Pipeline**
- Real-time data ingestion dengan Kafka
- Spark Streaming processing
- Real-time analytics
- Dashboard visualization

**Option 3: Hybrid Pipeline**
- Batch + Stream processing
- Lambda architecture
- Historical + real-time data

## 📝 Project Structure

### Part 1: Data Ingestion (20 points)

**Requirements:**
- Multiple data sources (min 2)
- Data validation
- Error handling
- Monitoring logs

**Deliverables:**
- Ingestion scripts
- Configuration files
- Documentation

### Part 2: Data Processing (30 points)

**Requirements:**
- Spark/MapReduce processing
- Data cleaning
- Transformations
- Aggregations
- Optimized performance

**Deliverables:**
- Processing scripts
- Performance benchmarks
- Execution logs

### Part 3: Data Storage (20 points)

**Requirements:**
- HDFS/S3 storage
- Appropriate file format (Parquet/ORC)
- Partitioning strategy
- Metadata management

**Deliverables:**
- Storage schema
- Data layout documentation
- Sample data

### Part 4: Pipeline Automation (15 points)

**Requirements:**
- Automated workflow
- Error recovery
- Scheduling (cron/Airflow)
- Monitoring

**Deliverables:**
- Automation scripts
- Scheduler configuration
- Monitoring dashboard

### Part 5: Documentation (15 points)

**Requirements:**
- Architecture diagram
- Setup instructions
- Usage guide
- Performance analysis
- Challenges & solutions

**Deliverables:**
- README.md
- Technical documentation
- Presentation slides

## ✅ Rubrik Penilaian

| Komponen | Points | Kriteria |
|----------|--------|----------|
| **Data Ingestion** | 20 | Multiple sources, validation, error handling |
| **Data Processing** | 30 | Spark implementation, transformations, optimization |
| **Data Storage** | 20 | File format, partitioning, schema design |
| **Automation** | 15 | Workflow orchestration, monitoring |
| **Documentation** | 15 | Clear, complete, professional |
| **Code Quality** | 10 | Clean, commented, organized |
| **Presentation** | 10 | Demo, explanation, Q&A |
| **TOTAL** | **120** | **Normalized to 100** |

## 📤 Deliverables

### 1. Source Code
```
NIM_Nama_UTS/
├── ingestion/
│   ├── kafka_producer.py
│   └── batch_loader.py
├── processing/
│   ├── spark_job.py
│   └── mapreduce/
├── storage/
│   └── schema.sql
├── automation/
│   ├── pipeline.sh
│   └── airflow_dag.py
├── config/
│   └── config.yaml
├── docs/
│   ├── README.md
│   ├── architecture.pdf
│   └── presentation.pptx
└── data/
    └── sample_data/
```

### 2. Documentation

**README.md must include:**
- Project overview
- Architecture diagram
- Prerequisites
- Setup instructions
- How to run
- Sample commands
- Performance results
- Challenges faced

### 3. Demo Video (Optional, +10 bonus)
- 5-10 minutes
- Show pipeline execution
- Explain components
- Demonstrate results

## 📊 Example Projects

### Example 1: E-commerce Analytics
- Ingest: Sales transactions (CSV, API)
- Process: Daily/monthly aggregations
- Storage: Partitioned by date
- Output: Sales dashboard

### Example 2: Social Media Stream
- Ingest: Twitter/Reddit stream via Kafka
- Process: Sentiment analysis, trending topics
- Storage: Real-time + batch views
- Output: Real-time dashboard

### Example 3: IoT Sensor Pipeline
- Ingest: Sensor data stream
- Process: Anomaly detection
- Storage: Time-series database
- Output: Alert system

## 💡 Tips Sukses

### Technical
1. Start simple, then enhance
2. Test components individually
3. Use Docker for consistency
4. Monitor resource usage
5. Implement error handling

### Documentation
1. Architecture diagram is crucial
2. Include sample commands
3. Screenshot key components
4. Explain design decisions

### Time Management
- Week 1: Planning & setup
- Week 2: Implementation
- Week 3: Testing & documentation
- Week 4: Finalization & presentation

## 🚫 Common Mistakes

1. ❌ Overly complex architecture
2. ❌ No error handling
3. ❌ Poor documentation
4. ❌ Tidak bisa di-reproduce
5. ❌ Missing performance analysis

## ✅ Best Practices

1. ✅ Modular code structure
2. ✅ Configuration files (not hardcoded)
3. ✅ Logging and monitoring
4. ✅ Version control (Git)
5. ✅ Performance benchmarks

## 📚 Resources

- [Apache Spark Examples](https://github.com/apache/spark/tree/master/examples)
- [Kafka Tutorials](https://kafka.apache.org/documentation/#gettingStarted)
- [Data Pipeline Patterns](https://www.oreilly.com/library/view/designing-data-intensive-applications/9781491903063/)

## 📧 Submission

**Deadline:** [Sesuai jadwal - Pertemuan 8]

**Format:** `NIM_Nama_UTS_DataPipeline.zip`

**Upload:** LMS or GitHub repository link

**Late Submission:**
- 1 day: -10 points
- 2 days: -20 points
- 3+ days: -50 points

## ❓ FAQ

**Q: Boleh pakai cloud services (AWS/GCP)?**
A: Boleh, tapi jelaskan setup-nya dan sediakan alternative untuk local testing.

**Q: Minimum data size?**
A: Batch: > 1GB, Streaming: > 10,000 messages

**Q: Boleh kerja kelompok?**
A: Tidak. Individual project.

**Q: Apakah harus real production data?**
A: Tidak. Boleh synthetic/sample data, asal realistic.

---

## 🎯 Final Checklist

Sebelum submit:

- [ ] All components working end-to-end
- [ ] Error handling implemented
- [ ] Documentation complete
- [ ] Architecture diagram included
- [ ] Performance analysis done
- [ ] Code clean and commented
- [ ] Can be reproduced from scratch
- [ ] Demo prepared

---

**Good Luck! Build an Awesome Data Pipeline! 🚀📊**

**Remember:** Ini adalah showcase dari semua yang sudah dipelajari di pertemuan 1-7. Tunjukkan best effort Anda!

---

**Questions?** Contact asisten praktikum atau diskusikan di forum kelas.
