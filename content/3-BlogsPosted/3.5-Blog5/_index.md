---
title: "Blog 5: Modernizing Fraud Claims with EMR"
date: 2026-07-19
weight: 5
chapter: false
pre: " <b> 3.5. </b> "
description: "How MAPFRE USA built an automated, serverless big data pipeline with Amazon EMR Serverless and Apache Iceberg to modernize fraud detection."
tags: ["AWS", "Big Data", "EMR Serverless", "Apache Iceberg", "Machine Learning"]
---

# How MAPFRE USA Modernized Fraud Claims with Amazon EMR Serverless

*This post shares my technical insights and reflections as a student intern in the **AWS First Cloud AI Journey** program, diving into a massive real-world serverless architecture solution.*

---

## 📌 The Journey Begins

As an intern in the **AWS First Cloud AI Journey** program, I am always excited to see how Cloud Computing intersect with **Artificial Intelligence and Big Data**. In school, we learn about data processing in isolation—write a python script, run it on a CSV file, and train a model. But in the real world, insurance companies like **MAPFRE USA** have to scan millions of insurance claims, mapping complex networks of suspicious relationships (like shared phone numbers or vehicle registrations) to detect fraud rings in real-time. Discovering how MAPFRE USA combined serverless big data processing with graph databases and explainable AI was an absolute revelation. It's incredibly inspiring to see how these pieces fit together to protect honest customers and stop fraud!

---

## 🏛️ Original Architecture Deep-Dive

MAPFRE USA modernized its fraud detection platform by moving from on-premises legacy servers to an automated, cloud-native big data and AI pipeline on AWS.

### Context & Challenges
Detecting modern insurance fraud requires processing huge volumes of unstructured and structured data:
* **Complex Data Relationships:** Traditional SQL databases struggle to model and query interconnected fraud rings (e.g., a network of claimants sharing the same lawyer, doctor, and phone number).
* **Operational Overhead:** Managing on-premises Hadoop or Spark clusters for big data workloads required dedicated administration teams and led to wasted idle server costs.
* **Explainability Deficit:** When machine learning models flag a claim as fraudulent, insurance investigators need clear, legal explanations of *why* it was flagged, not just a black-box score.

### Architectural Layout
MAPFRE USA implemented an end-to-end data lakehouse and ML pipeline composed of:
* **Amazon S3 & Apache Iceberg:** Used as the core data lakehouse storage layer, allowing ACID transactions and schema evolution over millions of records.
* **Amazon EMR Serverless (Apache Spark):** Processes large-scale ETL workloads, performs feature engineering, and runs model training jobs without managing any server infrastructure.
* **Amazon MWAA (Managed Workflows for Apache Airflow):** Orchestrates the entire data pipeline from ingestion to scoring.
* **Neo4j Graph Database:** Models relationships between claimants, vehicles, addresses, and phone numbers to visualize fraud rings.
* **SageMaker & SHAP (SHapley Additive exPlanations):** Computes feature importance scores to explain ML decisions to human investigators.

![MAPFRE USA Fraud Claims Modernization Architecture Diagram](/images/3-BlogsPosted/blog4-emr-serverless-fraud.png)

---

## 🛠️ Deep Academic Analysis & Technical Highlights

To comprehend how MAPFRE USA operates this enterprise pipeline, we must inspect the integration points of the lakehouse format and machine learning:

### 1. Apache Iceberg Metadata Architecture on Amazon S3
Standard data lakes organize data in folders on S3 and scan directories to query files. When a table has millions of rows, scanning directories degrades query performance significantly.
**Apache Iceberg** solves this by using a hierarchical metadata tree:
1. **Iceberg Catalog:** Points to the current Metadata File.
2. **Metadata File:** Stores table schema details, partition definitions, and points to a list of Manifest Lists.
3. **Manifest List:** Tracks snapshots of the table and links to individual Manifest Files.
4. **Manifest File:** Identifies the actual parquet data files on S3.
This architecture enables ACID transactions, fast partition pruning, and time travel queries, letting EMR Serverless Spark jobs read and write safely in parallel.

### 2. Amazon EMR Serverless Computing Mechanics
In self-managed EMR clusters on EC2, when a Spark job completes, the virtual servers remain active, consuming idle costs unless manually scaled down.
**Amazon EMR Serverless** abstracts the EC2 instances. Under the hood, it provisions execution containers on-demand. When the Spark job initiates:
* Compute containers are allocated based on pre-defined job limits.
* Once the job finishes, containers are instantly destroyed.
* This eliminates resource idle time and lowers data processing operational overhead by 40%.

### 3. Explainable Machine Learning (SHAP Values) in Production
Deep neural networks or gradient-boosted trees (XGBoost) output a probability score (e.g., $92\%$ fraud probability) but act as "black boxes." SagesMaker runs **SHAP (SHapley Additive exPlanations)** values, which are rooted in cooperative game theory. 
SHAP assigns each feature an importance value for a specific prediction:

$$\phi_i(f, x) = \sum_{S \subseteq F \setminus \{i\}} \frac{|S|!(|F| - |S| - 1)!}{|F|!} \Big( f(S \cup \{i\}) - f(S) \Big)$$

This math quantifies the exact contribution of each feature (e.g., `Shared IP Address` contributed $+20\%$, `Time of Claim` contributed $+10\%$) to the final score, giving human investigators a legally-sound, transparent explanation.

| Challenge | Solution | Key Learning |
| :--- | :--- | :--- |
| **Managing Spark clusters** | Replaced self-managed EC2 clusters with **Amazon EMR Serverless**. | Serverless Spark scales compute resources automatically, eliminating idle costs and reducing admin tasks. |
| **Data consistency on S3** | Adopted the **Apache Iceberg** table format on S3. | Iceberg enables reliable concurrent writes and snapshot isolation, turning S3 into a powerful data lakehouse. |
| **Detecting network fraud** | Integrated **Neo4j graph database** into the pipeline. | Graphs are superior to relational tables when querying complex, multi-degree relationships. |
| **Black-box ML predictions** | Integrated **SHAP values** into the ML inference step. | Explainable AI is crucial at trust boundaries, helping fraud investigators understand model decisions. |

---

## 💡 Reflection & Internship Lessons

This architecture case study was a masterclass in modern data engineering for me.

Here are the key takeaways from my perspective:
1. **Serverless Big Data is the Future:** I used to think running Apache Spark required hours of cluster configuration, choosing EC2 instance types, and managing node scaling. EMR Serverless shows that you can run massive Spark jobs instantly with zero management overhead.
2. **Iceberg is Transforming Storage:** Learning how Apache Iceberg brings database-like features (ACID, schema evolution, time travel) to object storage (S3) has changed my view of data lakes.
3. **AI Needs Human Trust:** This was my favorite part of the blog. It's not enough to build a highly accurate ML model; we must build *explainable* models. Using SHAP values to explain the "why" behind the "what" is a critical requirement for real-world AI deployment.

Getting to study such advanced architectures makes me incredibly eager to experiment with AWS Glue, EMR, and SageMaker during my **AWS First Cloud AI Journey**. It shows that with AWS, even a student can build enterprise-grade data platforms!

---

## 🔗 References
* [Original AWS Architecture Blog: How MAPFRE USA modernized fraud claims with Amazon EMR Serverless](https://aws.amazon.com/blogs/architecture/how-mapfre-usa-modernized-fraud-claims-with-amazon-emr-serverless/)
* [Amazon EMR Serverless User Guide](https://docs.aws.amazon.com/emr/latest/EMR-Serverless-User-Guide/emr-serverless.html)
* [Apache Iceberg Documentation](https://iceberg.apache.org/)
