🚀 Stratus Self-Service Streaming Platform (Azure)
📌 Overview

Implemented a config-driven, self-service real-time streaming ingestion platform on Azure using Event Hubs (Kafka API), Databricks, and Delta Lake.

The platform enables onboarding of new CDC-based data pipelines without code changes, supporting replay-safe, idempotent, and scalable processing across multiple entities.

Designed with production-grade patterns including multi-layer lakehouse architecture (Bronze/Silver/Gold), schema validation, deduplication, and CDC merge logic.

🏗️ Architecture
Producer (CDC Events)
        ↓
Azure Event Hubs (Kafka API)
        ↓
Databricks Bronze (Raw Streaming Ingestion)
        ↓
Databricks Silver (Parse + Validate + Deduplicate)
        ↓
Databricks Gold (CDC Merge: Insert / Update / Delete)
        ↓
Delta Lake (ADLS Gen2 / Volumes)
⚙️ Tech Stack
Streaming: Azure Event Hubs (Kafka API)
Processing: PySpark, Spark Structured Streaming
Compute: Azure Databricks
Storage: ADLS Gen2 / Databricks Volumes
Table Format: Delta Lake
Language: Python
🔁 Pipeline Flow
CDC events are produced using Python producers.
Events are ingested into Azure Event Hubs.
Bronze Layer
Raw ingestion from Event Hubs
Schema applied minimally
Stored as Delta tables
Silver Layer
JSON parsing and schema validation
Deduplication using event keys
Data quality checks applied
Gold Layer
CDC merge logic implemented (INSERT, UPDATE, DELETE)
Upserts handled using Delta Lake MERGE
🧠 Key Engineering Design Decisions
1. Config-Driven Architecture
Each entity (e.g., customer, orders) is defined via JSON config
Enables zero-code onboarding of new pipelines
Improves scalability and reduces operational overhead
2. Lakehouse Layering (Bronze / Silver / Gold)
Bronze → immutable raw ingestion (audit + replay)
Silver → cleaned, validated, deduplicated data
Gold → business-ready CDC merged tables

👉 Ensures separation of concerns and fault isolation

3. Idempotent Processing
Events processed using event_id + business keys
Deduplication handled in Silver layer
Prevents duplicate writes during retries or replays
4. Replay Capability
Streaming checkpoints stored in Databricks
Bronze acts as source of truth
Pipelines can be reprocessed from Bronze safely
5. CDC Handling Strategy
Operation column (I, U, D) used
Gold layer applies MERGE INTO Delta tables
Supports full lifecycle: insert, update, delete
📈 Scalability Considerations

Designed to scale for high-throughput streaming workloads:

Event Hubs partitions enable parallel ingestion
Spark Structured Streaming supports micro-batch parallelism
Delta Lake optimized for large-scale data processing

Target capability (design-level):

Millions of events/day
Low-latency ingestion pipeline
Horizontal scaling via Spark clusters
🛡️ Reliability & Fault Tolerance
Checkpointing ensures exactly-once processing semantics (Spark-level)
Delta Lake ACID guarantees prevent partial writes
Failures recover automatically from last checkpoint
Bronze layer allows full replay in case of corruption
🔄 Schema Handling
Schema enforced at Silver layer
Supports controlled schema evolution via config updates
Invalid records can be isolated during validation phase
🧪 Implemented Entities
customer
orders

Each entity follows full CDC lifecycle with independent configs.

⚡ Features
Config-driven onboarding for new entities
Real-time streaming ingestion
Schema validation and parsing
Deduplication and idempotency
CDC merge handling (insert/update/delete)
Multi-entity orchestration
Replay-safe architecture
🧩 Project Structure
notebooks/   → Databricks pipeline notebooks (Bronze, Silver, Gold)
configs/     → Entity-specific JSON configs
producer/    → CDC event producers (Python)
architecture/→ Architecture diagrams + execution proof screenshots
🧾 Azure & Databricks Proof

✔ Azure Event Hubs namespace and Kafka endpoint configured
✔ Databricks workspace with streaming pipelines
✔ Successful execution of Bronze, Silver, and Gold layers
✔ CDC merge validation with insert/update/delete operations

(Screenshots included in /architecture)

🧠 How to Onboard a New Entity
Add new JSON config in /configs
Define schema + primary keys + CDC fields
Trigger orchestration notebook
Pipeline automatically processes new entity end-to-end

👉 No code change required

✅ Result

Successfully built and validated an end-to-end real-time CDC streaming platform on Azure with:

Replay-safe pipelines
Idempotent processing
Multi-entity support
Production-aligned architecture patterns
