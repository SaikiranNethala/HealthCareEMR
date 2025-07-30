
# Ultimate Azure Data Engineering Project - Healthcare Revenue Cycle Management (RCM)

This project simulates a real-world **Azure Data Engineering** scenario for the **Healthcare RCM domain** using the **Azure Data Engineering stack**. The solution is designed to build robust, scalable, and secure data pipelines from multiple sources into a medallion architecture (Bronze → Silver → Gold).

---

## 🚀 Project Overview

- **Domain**: Healthcare - Revenue Cycle Management (RCM)
- **Technology Stack**: Azure Data Factory, Azure Databricks, ADLS Gen2, Azure SQL DB, Delta Lake, Key Vault
- **Architecture**: Medallion Architecture (Bronze, Silver, Gold)

---

## 🏥 About Revenue Cycle Management (RCM)

RCM is the financial process used by healthcare facilities to track patient care episodes from registration and appointment scheduling to the final payment of a balance.

### RCM Flow:
1. **Patient Visit** → Collect details, verify insurance
2. **Service Provided**
3. **Billing** → Bill creation
4. **Claims Review** → By insurance
5. **Payment & Follow-up** → Remaining paid by patient
6. **Tracking KPIs**:
   - **AR > 90 Days** (e.g. 20%)
   - **Days in AR** (e.g. 45 Days)

---

## 📁 Data Sources

| Source         | Format          | Storage              |
|----------------|------------------|-----------------------|
| EMR Data       | Azure SQL DB     | Bronze (Parquet)      |
| Claims         | Flat Files (CSV) | Landing → Bronze      |
| NPI / ICD API  | JSON (API)       | Bronze (Parquet)      |
| CPT Codes      | Flat Files (CSV) | Landing → Bronze      |

---

## 🧱 Architecture: Medallion Pattern

```
Landing Zone → Bronze (Raw) → Silver (Cleaned/CDM/SCD2) → Gold (Facts/Dimensions)
```

### 🟫 Bronze Layer
- **Storage Format**: Parquet
- **Goal**: Store raw ingested data from all sources
- **Tools**: ADF for EMR/Claims/CPT, API calls for NPI/ICD

### ⚪ Silver Layer
- **Storage Format**: Delta Tables
- **Goals**:
  - Apply cleaning rules
  - Common Data Model (CDM)
  - Implement SCD Type 2
  - Perform Quality Checks (`is_quarantined`)

### 🟨 Gold Layer
- **Storage Format**: Delta Tables
- **Goal**: Create Fact and Dimension tables for Business Intelligence and reporting

---

## 🧩 Data Engineering Tasks

### ✅ Bronze Layer Tasks
- Ingest EMR (SQL DB → Parquet)
- Ingest Claims/CPT (Landing CSV → Parquet)
- Ingest ICD/NPI (API → Parquet)

### ✅ Silver Layer Tasks
- Clean and transform data to CDM
- Apply SCD2 (on Patients, Transactions, etc.)
- Implement Quality Checks (`is_quarantined`)
- Store as Delta Tables

### ✅ Gold Layer Tasks
- Create KPIs and metrics
- Develop Fact and Dimension Tables

---

## 🧪 Sample KPI Calculations

- **% AR > 90 Days**: `200K / 1M = 20%`
- **Days in AR**: `$400K AR / $10K per day = 40 Days`

---

## ⚙️ Tools & Services Used

- **Azure Data Factory (ADF)**: Data Ingestion
- **Azure SQL Database**: EMR data source
- **Azure Data Lake Storage Gen2**: Landing → Bronze → Silver → Gold
- **Azure Databricks**: Data Processing & Transformations
- **Azure Key Vault**: Secrets Management
- **Delta Lake**: For Silver and Gold layers
- **Unity Catalog** *(Optional)*: Table-level security (upgrade from Hive metastore)

---

## 📂 Folder Structure in ADLS (ttadlsdev)

```
ttadlsdev/
│
├── landing/
│   ├── claims/
│   └── cpt/
├── bronze/
│   ├── hosa/
│   ├── hosb/
│   └── codes/
├── silver/
├── gold/
└── configs/
    └── emr/
        └── load_config.csv
```

---

## 📜 ADF Pipeline Components

- **Linked Services**:
  - Azure SQL DB
  - ADLS Gen2
  - Delta Lake
  - Azure Key Vault
  - Azure Databricks

- **Datasets**:
  - Azure SQL Tables
  - CSV Configs
  - Parquet (Bronze)
  - Delta Tables (Silver, Gold)

- **Activities**:
  - Lookup (Config file)
  - ForEach (Loop over tables)
  - Copy (Full & Incremental)
  - Stored Procedure (Audit logs)

---

## 🛡️ Security

- Secrets like access keys are managed via **Azure Key Vault**
- Integration with Databricks using **dbutils.secrets.get()**

---

## 🔁 Enhancements Done

- ✅ Parallelized ADF Pipeline
- ✅ Implemented Key Vault
- ✅ Refined naming conventions
- ✅ Audit logging in Delta Tables
- ✅ Active/Inactive flags
- ✅ Added Retry policies

---

## 🔍 SCD Type 2 Fields

```sql
inserted_date TIMESTAMP,
modified_date TIMESTAMP,
is_current BOOLEAN
```

---

## 📘 References

- [ICD vs CPT Codes](https://www.simplepractice.com/blog/icd-codes-and-cpt-codes/)
- [Azure Medallion Architecture](https://learn.microsoft.com/en-us/azure/architecture/example-scenario/dataplat/medallion-architecture)

---

## 👨‍💻 Author

**Sumit Sir (TrendyTech)**
- 📍 Hyderabad
- 🌐 [TrendyTech Website](https://trendytech.in)

---

## 🧾 License

This project is for educational purposes only. All data used is synthetically generated using Faker.

---

## 📦 Clone This Repository

```bash
git clone https://github.com/your-username/azure-healthcare-rcm-pipeline.git
```

---
