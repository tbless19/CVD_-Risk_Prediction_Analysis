# Distributed Cardiovascular Disease Risk Prediction using Apache Spark

## Overview
This project employs **Apache Spark** for distributed data preprocessing, exploratory data analysis (EDA), and machine learning to predict **cardiovascular disease (CVD)** risk.  
Using the **Framingham Heart Study dataset**, we demonstrate how parallel computing across multiple virtual machines (VMs) significantly enhances computational efficiency and reduces total processing time for predictive analytics.

---

## Objectives
- Implement **Apache Spark** on a multi-VM cluster for distributed data processing.  
- Compare **single-node vs. multi-node** performance in preprocessing and model training.  
- Build and evaluate **machine learning models** for ten-year CHD risk prediction.  
- Demonstrate scalability and efficiency improvements using parallelized computation.

---

## Dataset Information
**Source:** [Framingham Heart Study Dataset (Kaggle)](https://www.kaggle.com/datasets)  
**Records:** ~4,000  
**Features:** 15  
**Target:** `TenYearCHD` (0 = No CHD, 1 = CHD)

| Type | Example Features |
|------|------------------|
| Demographic | Age, Sex |
| Behavioral | Smoking, BMI |
| Clinical | Cholesterol, Blood Pressure, Glucose |

---

## System Configuration

### VPN & Access
- Connected to **Michigan Tech VPN** via *BIG-IP Edge Client*  
- Accessed **VCenter** at: `https://vcenter-ccc.ad.mtu.edu/`

### ⚙️ Virtual Machine Setup
Each VM was created and configured in the VCenter environment.

| VM | Role | Example IP Address |
|----|------|---------------------|
| hadoop1 | Master | 192.168.13.164 |
| hadoop2 | Worker | 192.168.13.165 |

> Multiple IPs were tested to ensure network connectivity across all nodes.

### 🧮 Spark Cluster Configuration

**Generate SSH key pair (on hadoop1):**
```bash
ssh-keygen -t rsa
cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

**Copy key to worker node:**
```bash
scp ~/.ssh/id_rsa.pub root@hadoop2:/root/.ssh/authorized_keys
```

**Deploy Spark to both nodes:**
```bash
# On hadoop1
cd /opt
tar czf spark.tar.gz spark
scp spark.tar.gz root@hadoop2:/opt

# On hadoop2
cd /opt
tar xvzf spark.tar.gz
```

**Start Spark Cluster:**
```bash
# Start master
/opt/spark/sbin/start-master.sh

# Start specific worker
/opt/spark/sbin/start-worker.sh spark://hadoop1:7077

# Start all (master + workers)
/opt/spark/sbin/start-all.sh
```

**Submit Spark job:**
```bash
/opt/spark/bin/spark-submit --master spark://hadoop1:7077 /opt/preprocessing.py
```

---

## ⚗️ Methodology

| Preprocessing Method | ML Model | VMs Used | Purpose |
|----------------------|-----------|-----------|----------|
| KNN Imputation | Logistic Regression | 1–2 | Handle missing data |
| Z-Score Outlier Removal | Random Forest | 3–4 | Improve robustness |
| Min–Max Normalization | Gradient Boosting | 5–6 | Scale features |
| Chi-Square Feature Selection | SVM (RBF) | 7–8 | Enhance discriminative features |
| PCA Dimensionality Reduction | Neural Network (MLP) | 9–10 | Capture hidden patterns |

Each participant used the same dataset but applied different preprocessing and model pipelines for comparative analysis.

---

## 📊 Performance Evaluation

| Model | Accuracy (%) | ROC-AUC |
|--------|---------------|----------|
| Logistic Regression | 84.67 | 0.525 |
| Random Forest | 84.59 | 0.518 |
| Gradient Boosting | 84.59 | 0.543 |
| SVM (RBF) | 85.06 | 0.512 |
| Neural Network (MLP) | 78.38 | 0.553 |

---

## Performance Comparison

- **Single VM:** Highest runtime due to limited resources.  
- **2–4 VMs:** Significant performance improvement with distributed tasks.  
- **5+ VMs:** Further speedup observed but diminishing returns due to network overhead.  
- **Result:** Multi-node Spark execution achieved up to **5× faster** performance than single-node execution.

---

## Challenges Encountered
- Synchronization and port-access issues between VMs.  
- Occasional browser crashes (Firefox) during Spark UI monitoring.  
- Incomplete connectivity among all 10 VMs.  
- Memory management bottlenecks during large Spark actions.  
- Model tuning difficulties (especially for SVM and MLP convergence).

---

## Conclusion
This project demonstrates the successful deployment of a **distributed Spark cluster** for CVD risk prediction using the **Framingham dataset**.  
The results confirm that **parallel big-data computing** significantly accelerates preprocessing and model training.  
The framework establishes a foundation for **real-time, scalable, and interpretable** predictive healthcare analytics systems.

---]

## Tools & Technologies
- **Apache Spark**
- **Python 3.x**
- **PySpark**
- **Scikit-learn**
- **NumPy / Pandas**
- **Matplotlib**
- **Virtual Machines (VCenter Environment)**
