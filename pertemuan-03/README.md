# Pertemuan 3: MapReduce Fundamentals

## 🎯 Tujuan Pembelajaran

Setelah menyelesaikan pertemuan ini, mahasiswa diharapkan mampu:
1. Memahami paradigma pemrograman MapReduce
2. Mengimplementasikan Word Count dengan MapReduce
3. Memahami fase Map, Shuffle, dan Reduce
4. Mengoptimalkan MapReduce jobs dengan Combiner
5. Debugging dan monitoring MapReduce applications

## 📚 Teori Singkat

### Apa itu MapReduce?

MapReduce adalah programming model untuk processing large datasets secara distributed dan parallel.

**Workflow:**
```
Input Data
    │
    ▼
┌─────────┐
│   Map   │ → Transform to key-value pairs
└─────────┘
    │
    ▼
┌──────────────┐
│Shuffle & Sort│ → Group by key
└──────────────┘
    │
    ▼
┌─────────┐
│ Reduce  │ → Aggregate values
└─────────┘
    │
    ▼
 Output
```

### Word Count Example

**Input:** "Hello World Hello Hadoop"

**Map:** (Hello,1), (World,1), (Hello,1), (Hadoop,1)

**Shuffle:** (Hadoop,[1]), (Hello,[1,1]), (World,[1])

**Reduce:** (Hadoop,1), (Hello,2), (World,1)

## 📝 Praktikum

### Langkah 1: Word Count with Hadoop Streaming

**mapper.py:**
```python
#!/usr/bin/env python3
import sys

for line in sys.stdin:
    line = line.strip()
    words = line.split()
    
    for word in words:
        print(f"{word.lower()}\t1")
```

**reducer.py:**
```python
#!/usr/bin/env python3
import sys

current_word = None
current_count = 0

for line in sys.stdin:
    word, count = line.strip().split('\t')
    count = int(count)
    
    if word == current_word:
        current_count += count
    else:
        if current_word:
            print(f"{current_word}\t{current_count}")
        current_word = word
        current_count = count

if current_word:
    print(f"{current_word}\t{current_count}")
```

**Run:**
```bash
chmod +x mapper.py reducer.py

# Test locally
echo "Hello World Hello" | ./mapper.py | sort | ./reducer.py

# Run on Hadoop
hadoop jar $HADOOP_HOME/share/hadoop/tools/lib/hadoop-streaming-*.jar \
  -input /input \
  -output /output \
  -mapper mapper.py \
  -reducer reducer.py \
  -file mapper.py \
  -file reducer.py
```

### Langkah 2: With PySpark (Recommended)

```python
from pyspark import SparkContext

sc = SparkContext("local", "WordCount")

text_file = sc.textFile("input.txt")

counts = text_file \
    .flatMap(lambda line: line.split()) \
    .map(lambda word: (word.lower(), 1)) \
    .reduceByKey(lambda a, b: a + b)

counts.saveAsTextFile("output")
sc.stop()
```

## 💪 Tugas Praktikum

### Tugas 1: Enhanced Word Count (25%)
- Remove stop words & punctuation
- Count only words length > 3
- Output top 20 words

### Tugas 2: Sales Analysis (30%)
Dataset: (date, product, quantity, price)
- Total sales per product
- Monthly revenue trends

### Tugas 3: Log Analysis (25%)
- Requests per status code
- Top URLs visited
- Unique visitors by IP

### Tugas 4: Performance Benchmark (20%)
Compare: MapReduce vs Spark vs Python

## 📚 Referensi

1. [MapReduce Tutorial](https://hadoop.apache.org/docs/stable/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html)
2. [Hadoop Streaming](https://hadoop.apache.org/docs/stable/hadoop-streaming/HadoopStreaming.html)

---

**Happy MapReducing! 🗺️**
