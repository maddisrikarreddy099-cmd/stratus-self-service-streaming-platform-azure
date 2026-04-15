# 🚀 Stratus Self-Service Streaming Platform (Azure)

## 📌 Overview

Implemented a **config-driven, self-service real-time streaming ingestion platform** on Azure using **Event Hubs (Kafka API), Databricks, and Delta Lake**.

The platform enables onboarding of new CDC-based pipelines **without code changes**, supporting **replay-safe, idempotent, and scalable processing** across multiple entities.

Designed using production-grade patterns including **Bronze/Silver/Gold layering, schema validation, deduplication, and CDC merge logic**.

---

## 🏗️ Architecture


Producer (CDC Events)
↓
Azure Event Hubs (Kafka API)
↓
Databricks Bronze (Raw Ingestion)
↓
Databricks Silver (Parse + Validate + Deduplicate)
↓
Databricks Gold (CDC Merge: Insert / Update / Delete)
↓
Delta Lake (ADLS Gen2 / Volumes)


📷 Detailed architecture diagram available in `/architecture`

---

## ⚙️ Tech Stack

- **Streaming**: Azure Event Hubs (Kafka API)
- **Processing**: PySpark, Spark Structured Streaming
- **Compute**: Azure Databricks
- **Storage**: ADLS Gen2 / Databricks Volumes
- **Table Format**: Delta Lake
- **Language**: Python

---

## 🔁 Pipeline Flow

### 🟤 Bronze Layer
- Raw ingestion from Event Hubs
- Minimal schema enforcement
- Stored as Delta tables (append-only)

### ⚪ Silver Layer
- JSON parsing and schema validation
- Deduplication using event keys
- Data quality checks applied

### 🟡 Gold Layer
- CDC merge logic (`INSERT`, `UPDATE`, `DELETE`)
- Upserts handled using Delta Lake `MERGE`

---

## 🧠 Key Engineering Decisions

### ⚙️ Config-Driven Architecture
- Each entity defined via JSON config
- Enables **zero-code onboarding**
- Scales across multiple entities

### 🔁 Idempotent Processing
- Deduplication using `event_id` + business keys
- Prevents duplicate writes during retries

### 🔄 Replay Capability
- Bronze layer acts as **source of truth**
- Checkpoint-based recovery via Spark

### 🧩 CDC Handling Strategy
- Operation types: `I`, `U`, `D`
- Gold layer applies Delta `MERGE INTO`

---

## 📈 Scalability

Designed for high-throughput streaming workloads:

- Event Hubs partitions enable parallel ingestion
- Spark micro-batch processing enables horizontal scaling
- Delta Lake optimized for large-scale data processing

**Target Capability:**
- Millions of events/day
- Low-latency ingestion
- Distributed processing via Spark clusters

---

## 🛡️ Reliability & Fault Tolerance

- Checkpointing ensures **exactly-once processing (Spark-level)**
- Delta Lake provides **ACID guarantees**
- Automatic recovery from failures
- Bronze layer enables full replay

---

## 🔄 Schema Handling

- Schema enforced at Silver layer
- Supports controlled schema evolution via configs
- Invalid records can be isolated during validation

---

## 🧪 Implemented Entities

- `customer`
- `orders`

Each entity supports full CDC lifecycle independently.

---

## ⚡ Features

- Config-driven onboarding
- Real-time streaming ingestion
- Schema validation and parsing
- Deduplication and idempotency
- CDC merge handling (insert/update/delete)
- Multi-entity orchestration
- Replay-safe architecture

---

## 🧩 Project Structure


notebooks/ → Databricks pipeline notebooks (Bronze, Silver, Gold)
configs/ → Entity-specific JSON configs
producer/ → CDC event producer scripts
architecture/ → Architecture diagrams + proof screenshots


---

## 🧾 Azure & Databricks Proof

✔ Azure Event Hubs namespace and Kafka endpoint configured  
✔ Databricks workspace with streaming pipelines  
✔ Successful execution of Bronze, Silver, and Gold layers  
✔ CDC merge validation (insert/update/delete)  

📷 Screenshots available in `/architecture`

---

## 🧠 How to Onboard a New Entity

1. Add new JSON config in `/configs`
2. Define schema, primary keys, and CDC fields
3. Trigger orchestration notebook
4. Pipeline processes entity end-to-end

👉 **No code change required**

---

## ⚠️ Limitations & Future Improvements

- No Dead Letter Queue (DLQ) implemented
- Schema evolution partially manual
- Monitoring/alerting not integrated
- Cost optimization not tuned

**Planned Enhancements:**
- Add DLQ for failed events
- Integrate Azure Monitor / logging
- Introduce schema registry
- Optimize cluster auto-scaling

---

## ✅ Result

Successfully built and validated an **end-to-end real-time CDC streaming platform** on Azure with:

- Replay-safe pipelines  
- Idempotent processing  
- Multi-entity support  
- Production-aligned architecture patterns  

---

## 👤 Author

Srikar Reddy Maddi  
Senior Data Engineer | Streaming Systems | Lakehouse Architectures
