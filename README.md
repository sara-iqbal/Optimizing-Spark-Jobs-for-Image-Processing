#  Spark Optimization for Distributed Image Processing

###  Project Overview
This project focuses on architecting and optimizing a distributed big data pipeline for processing large-scale image datasets. Using **Apache Spark (PySpark)** on **Google Cloud Dataproc**, I investigated the performance trade-offs between different cluster configurations, data formats, and caching strategies.

The goal was to eliminate I/O bottlenecks and maximize resource utilization for Deep Learning data loading pipelines.

**Key Achievement:** achieved **2x higher throughput** and linear scalability by optimizing data serialization (TFRecord vs. JPEG) and cluster parallelization.

---

##  Tech Stack & Tools
* **Cloud Platform:** Google Cloud Platform (Dataproc, GCS)
* **Big Data Engine:** Apache Spark (PySpark)
* **Data Processing:** TensorFlow, NumPy
* **Visualization:** Matplotlib
* **Concepts:** Distributed Computing, ETL, Caching, Serialization, Bayesian Optimization

---

## Architecture & Methodology

### 1. Cluster Optimization (Scaling & Resource Utilization)
I deployed a **4-node Dataproc cluster** (1 Master, 3 Workers) to evaluate distributed performance against a single-node baseline.
* **Result:** Achieved balanced CPU utilization across all nodes, confirming efficient task distribution.
* **Disk I/O:** Parallel read/write operations were optimized to reduce bottlenecks during the data ingestion phase.

> **Visualization:**
<img width="1207" height="452" alt="cpu 1d 1" src="https://github.com/user-attachments/assets/40db1442-4f38-403b-ac27-81937a56986e" />

### 2. Performance Benchmarking: TFRecord vs. JPEG
I conducted a regression analysis to compare the scalability of loading raw **JPEG** images versus serialized **TFRecord** files.
* **JPEG:** Showed limited gains as batch sizes increased, hitting an I/O ceiling.
* **TFRecord:** Demonstrated **linear scalability** (steep regression slope), proving it is the superior format for high-throughput distributed pipelines.

> **Visualization:**
<img width="1489" height="1715" alt="task 2 d1" src="https://github.com/user-attachments/assets/9ec99b64-c22c-430b-83a6-0d364d6b1b7f" />

### 3. Caching & Network Optimization
Implemented Spark caching strategies to minimize redundant network calls to Google Cloud Storage (GCS).
**Uncached:** High CPU spikes (40% peak) and repetitive high-latency reads.
**Cached:** Stabilized CPU usage and **reduced network I/O overhead by ~30%**, significantly speeding up iterative processing

---

##  Key Results

| Metric | Single-Node Cluster | Multi-Node Cluster (Optimized) |
| :--- | :--- | :--- |
| **Network Traffic** | Minimal (No inter-node comms) | Balanced Data Distribution  |
| **Disk I/O** | Bottlenecked on single disk  | Parallel throughput across 4 nodes  |
| **Scalability** | Limited by single CPU/RAM | *Linearly Scalable** (with TFRecords)  |

---

## CherryPick-Inspired Optimization
Inspired by the *CherryPick* paper (Alipourfard et al., 2017), I applied **Bayesian optimization principles** to automate configuration tuning. Instead of manual trial-and-error, this approach models the relationship between cluster size, data format, and throughput to predict the most cost-effective configuration.

---

##  How to Run
1.  **Prerequisites:**
    * GCP Account with Dataproc API enabled.
    * Python 3.x & PySpark installed.
2.  **Clone the Repo:**
    ```bash
    git clone [https://github.com/sara-iqbal/spark-image-optimization.git](https://github.com/sara-iqbal/spark-image-optimization.git)
    ```
3.  **Run the Benchmark (PySpark):**
    ```bash
    spark-submit --master yarn --deploy-mode cluster src/benchmark_job.py
    ```

---

## 👤 Author
**Sara Iqbal**
* [GitHub](https://github.com/sara-iqbal)

---
*This project was completed as part of the Big Data Coursework at City, University of London.*
