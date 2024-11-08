# JADARA Mart Data Engineering Project

## Overview

This repository documents the **Data Engineering** aspects of the JADARA Mart project. As a **Data Engineer**, I focused on designing and implementing robust ETL pipelines, resolving schema integration challenges, and establishing a unified, efficient data architecture. This project involved processing diverse data sources, automating data pipelines, and building a scalable data warehouse to empower JADARA Mart's business intelligence efforts.

---

## Data Engineering Objectives

1. **Data Integration**: Harmonize data from SQL Server (physical stores) and MySQL (online stores) into a unified schema.
2. **ETL Automation**: Develop scalable and reusable ETL pipelines to support daily operations.
3. **Data Consistency**: Ensure data accuracy and integrity across landing, staging, and data warehouse layers.
4. **Performance Optimization**: Optimize data transformations, loading, and retrieval processes for analytics.

---

## System Architecture

### Source Systems
- **SQL Server**: Used for physical store data, including sales, inventory, and customer details.
- **MySQL**: Migrated online store data from SQL Server to MySQL for testing heterogeneous integration.

### Destination System
- **Unified Data Warehouse**: Designed to store harmonized data for analytics. Built with six dimensions (`Customer`, `Order`, `Inventory`, `Product`, `Vendor`, `Date`) and two fact tables (`Sales`, `Supply`).

---

## ETL Process Design

### Key Layers
1. **Landing Database**:
   - Standardized schema with metadata columns (`Hashcode`, `Source`, `Landing_Date`) for tracking and auditing.
   - All transformations logged and tracked for traceability.
2. **Staging Database**:
   - Applied Slowly Changing Dimensions (SCD) logic and incremental loading.
   - Columns added for historical tracking: `StartDate`, `EndDate`, and `IsCurrent`.
3. **Data Warehouse**:
   - Fact and dimension tables optimized for OLAP queries.
   - Surrogate keys used for consistency across relationships.

### ETL Workflow
Developed with **SQL Server Integration Services (SSIS)** and structured into modular packages:
- **Data Extraction**:
  - Extracted data using `ADO` and `OLE DB Sources`.
  - Resolved schema mismatches by aligning data types with advanced editor configurations.
- **Transformation**:
  - Created derived columns for metadata such as `Landing_Date`.
  - Implemented a hash-based change tracking mechanism.
  - Used `Union All` to merge data from disparate sources.
- **Loading**:
  - Leveraged **Fast Load** for efficient data insertion into SQL Server.
  - Dynamic pre- and post-SQL scripts for disabling/enabling foreign key constraints.

### Automation
- Configured **SQL Server Agent** jobs to schedule ETL processes.
- Implemented an automated **email notification package** to track ETL pipeline execution status, sending logs as attachments.

---

## Data Warehouse Schema Design

### Dimensions
- **DimProduct**: Maintained historical data with SCD Type 2.
- **DimDate**: Provided a hierarchical time-based dimension.
- **DimCustomer, DimVendor, DimInventory, DimOrder**: Stored normalized attributes for analytics.

### Fact Tables
- **FactSales**:
  - Grain: Individual sales transaction.
  - Columns: Quantity, Total Price, Rating.
- **FactSupply**:
  - Grain: Supply batch from vendor.
  - Columns: Request Date, Delivery Date, Defective Quantity.

---

## Data Engineering Challenges

1. **Heterogeneous Schema Integration**:
   - SQL Server and MySQL databases had different structures.
   - Resolved by designing a unified schema with consistent column mappings.
   - Example: Consolidated `Customer_ID` and `CustomerName` into a standardized `Customer` table with surrogate keys.

2. **Data Type Discrepancies**:
   - Applied SSIS data conversion for fields like `VARCHAR` (MySQL) → `NVARCHAR` (SQL Server).

3. **Foreign Key Violations**:
   - Used dynamic SQL to disable and re-enable constraints during ETL.

4. **Incremental Fact Loading**:
   - Configured **Lookup Transformations** in SSIS to prevent duplicate inserts.

---

## Advanced Features

### Change Data Capture
Implemented a hash-based change detection mechanism to identify updated records efficiently.

### Incremental Loading
- Used `Lookup Transformation` to check if records existed in the fact tables.
- Inserted new records, ensuring efficient and non-redundant loads.

### Staging Data History
- Added `StartDate`, `EndDate`, `IsCurrent` to track versioning in staging tables.

---

## Tools and Technologies

| **Category**           | **Tools/Technologies**      |
|-------------------------|-----------------------------|
| Databases              | SQL Server, MySQL, Oracle SQL Developer |
| ETL Development        | SSIS, Informatica           |
| Programming            | T-SQL, PL/SQL              |
| Scheduling and Automation | SQL Server Agent, SSIS Jobs |
| Analytics              | Power BI                   |

---

## Testing and Validation

- Comprehensive SQL test cases for:
  - Data integrity checks in landing, staging, and warehouse layers.
  - Validation of KPIs such as sales trends and inventory health.
- Automated reports generated during each ETL run for error tracking.

---

## Key Results

- Reduced ETL run time by 30% using optimized data flows.
- Achieved 100% consistency in multi-source integration.
- Enabled real-time analytics through seamless data pipeline execution.
