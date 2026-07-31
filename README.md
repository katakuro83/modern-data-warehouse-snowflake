# Modern Data Warehouse on Snowflake

> Designing and building a modern cloud data warehouse using **Snowflake**, **dbt**, and **Apache Airflow**.

![Project Status](https://img.shields.io/badge/Status-In%20Progress-blue)
![Snowflake](https://img.shields.io/badge/Snowflake-Data%20Warehouse-29B5E8)
![dbt](https://img.shields.io/badge/dbt-Transformation-orange)
![Airflow](https://img.shields.io/badge/Airflow-Orchestration-017CEE)

---

# About the Project

This repository documents my journey of designing and implementing a modern cloud based data warehouse using industry standard tools and best practices. This repository is part of my professional portfolio and serves as a hands on learning project focused on modern Data Engineering practices.

Rather than publishing a finished solution, I am building this project incrementally to demonstrate how a production style analytics platform is developed from raw data ingestion to an analytics ready dimensional model. The goal is to design and implement a complete ELT pipeline from raw data ingestion to a dimensional star schema ready for analytics.

The project is being developed step by step as a portfolio project to demonstrate practical Data Engineering and Analytics Engineering skills.

Each milestone represents a new concept, technology, or architectural decision that contributes to a complete end-to-end ELT solution.

---

# Project Goals

This project demonstrates practical experience with:

* Modern Data Warehouse architecture
* ELT pipeline development
* Cloud data platforms
* Dimensional data modeling (Kimball)
* Data transformation with dbt
* Workflow orchestration with Apache Airflow
* SQL and Python development
* Data quality validation
* Documentation and version control

---

# Architecture

```
               Raw Data (CSV / API)
                       │
                       ▼
             Snowflake RAW Layer
                       │
                       ▼
             dbt Staging Models
                       │
                       ▼
          Analytics Star Schema
                       │
                       ▼
                 BI / Reporting
```

---

# Technology Stack

| Technology     | Purpose                |
| -------------- | ---------------------- |
| Snowflake      | Cloud Data Warehouse   |
| dbt            | Data Transformation    |
| Apache Airflow | Workflow Orchestration |
| SQL            | Data Modeling          |
| Python         | Data Ingestion         |
| Git & GitHub   | Version Control        |

---

# Planned Data Model

The final warehouse will follow the Kimball dimensional modeling approach.

```
               dim_customers
                     │
                     │
dim_date ─────── fact_orders ─────── dim_products
```

---

# Repository Structure

```
modern-data-warehouse-snowflake/

├── architecture/
├── airflow/
├── data/
│   └── raw/
├── dbt_project/
│   ├── models/
│   ├── tests/
│   └── macros/
├── docs/
├── ingestion/
├── sql/
└── README.md
```

---

# Project Roadmap

## Phase 1  Repository Setup

* [x] Create GitHub repository
* [x] Prepare project documentation
* [ ] Create folder structure

## Phase 2  Snowflake

* [ ] Create database
* [ ] Configure warehouse
* [ ] Create schemas
* [ ] Load sample data

## Phase 3  Data Ingestion

* [ ] Prepare CSV datasets
* [ ] Build Python ingestion script
* [ ] Load data into RAW layer

## Phase 4  dbt

* [ ] Configure dbt project
* [ ] Create staging models
* [ ] Build dimensional models
* [ ] Add tests

## Phase 5  Airflow

* [ ] Create DAG
* [ ] Automate ELT pipeline

## Phase 6  Documentation

* [ ] Generate dbt documentation
* [ ] Add architecture diagrams
* [ ] Publish project screenshots

---

# Current Status

**Work in Progress**
**This project is actively under development.**

The repository will evolve step by step as new components are implemented. Every major milestone will be documented and version-controlled to reflect a realistic software development workflow.

---

# Future Improvements

Planned enhancements include:

* Snowpipe for continuous ingestion
* Change Data Capture (CDC)
* Slowly Changing Dimensions (SCD Type 2)
* CI/CD with GitHub Actions
* Data Quality Monitoring
* Power BI dashboard integration
* Performance optimization
* Cost optimization in Snowflake

---
