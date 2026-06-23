# 001 - System Design

# Credit Risk Lakehouse

## 1. Purpose

Build a local credit risk data platform using PySpark, Docker, Jupyter Notebook and VS Code.

The project simulates a multiproduct credit portfolio and covers:

- Data ingestion
- Lakehouse architecture
- Feature engineering
- PD modeling
- Expected loss calculation
- Model monitoring
- Spark performance studies

## 2. Business Context

The platform represents a financial institution that offers different credit products.

Products:

- Personal Loan
- Credit Card
- Vehicle Financing
- Payroll Loan

Each product has different risk behavior, LGD assumptions, default probability and business rules.

## 3. Main Objective

The main goal is to build a professional portfolio project that demonstrates knowledge in:

- Big Data
- PySpark
- Data Engineering
- Lakehouse architecture
- Credit risk
- Machine Learning
- Model monitoring
- Scalable architecture

## 4. Architecture

Flow:

Raw Data  
↓  
Bronze Layer  
↓  
Silver Layer  
↓  
Gold Layer  
↓  
Feature Store  
↓  
PD Model  
↓  
Scoring  
↓  
Expected Loss / PDD  
↓  
Model Monitoring  

## 5. Data Layers

### 5.1 Raw

Stores generated source files exactly as they arrive.

### 5.2 Bronze

Stores raw data converted to Parquet with minimal transformation.

Purpose:

- Preserve original data
- Enable reprocessing
- Maintain traceability

### 5.3 Silver

Stores cleaned, typed and standardized datasets.

Responsibilities:

- Remove duplicates
- Apply schemas
- Standardize dates
- Normalize categorical values
- Validate business rules

### 5.4 Gold

Stores analytical datasets for credit risk analysis.

Examples:

- Portfolio by product
- Default rate by cohort
- Vintage analysis
- PD by segment
- PDD by product

### 5.5 Feature Store

Stores model-ready features.

Examples:

- Average delay in the last 6 months
- Maximum delay in the last 12 months
- Number of active contracts
- Bureau score
- Debt-to-income ratio
- Historical default flag

### 5.6 Monitoring

Stores model and data monitoring outputs.

Examples:

- PSI
- AUC
- KS
- Gini
- Bad Rate
- Default Rate
- Score distribution

## 6. Core Entities

Main datasets:

- Customers
- Credit Bureau
- Credit Applications
- Contracts
- Installments
- Payments
- Default Events

## 7. Credit Risk Metrics

### 7.1 PD

Probability of Default.

Represents the probability that a customer will default within a defined time horizon.

### 7.2 LGD

Loss Given Default.

Represents the percentage of exposure expected to be lost if default occurs.

### 7.3 EAD

Exposure at Default.

Represents the outstanding exposure at the time of default.

### 7.4 Expected Loss

Expected Loss = PD * LGD * EAD

### 7.5 PDD

Provision for expected credit losses.

## 8. Initial Product Assumptions

| Product | Risk Level | Initial LGD |
|---|---:|---:|
| Personal Loan | High | 75% |
| Credit Card | High | 85% |
| Vehicle Financing | Medium | 35% |
| Payroll Loan | Low | 15% |

## 9. Spark Learning Goals

This project must demonstrate understanding of:

- SparkSession
- DataFrame API
- Schemas
- Transformations
- Actions
- Lazy evaluation
- DAG execution
- Catalyst Optimizer
- Shuffle
- Joins
- Broadcast Join
- Window Functions
- Partitioning
- Repartition vs Coalesce
- Cache and Persist
- Explain Plans
- Data Skew

## 10. Technical Principles

- Do not use Spark as a bigger Pandas
- Avoid collecting large datasets to the driver
- Prefer native Spark functions over UDFs
- Use Parquet for analytical storage
- Document architectural decisions
- Design locally, but with scalability in mind
- Keep notebooks for exploration
- Keep src/ for production-like code

## 11. Repository Strategy

The repository must be organized as a professional portfolio project, not as a collection of loose notebooks.

Expected structure:

credit-risk-lakehouse/
├── docker/
├── data/
│   ├── raw/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   ├── feature_store/
│   └── monitoring/
├── docs/
│   ├── architecture/
│   ├── data_model/
│   ├── business_rules/
│   └── spark_studies/
├── notebooks/
├── src/
├── tests/
├── docker-compose.yml
├── Makefile
├── requirements.txt
├── pyproject.toml
└── README.md

## 12. Roadmap

### Sprint 0 - Architecture

Define system design, data model, business rules and repository structure.

### Sprint 1 - Environment

Set up Docker, Spark, PySpark, Jupyter Notebook and VS Code.

### Sprint 2 - Spark Fundamentals

Study SparkSession, DataFrames, schemas, transformations, actions and lazy evaluation.

### Sprint 3 - Bronze Layer

Ingest raw data into Parquet.

### Sprint 4 - Silver Layer

Clean, type, deduplicate and validate datasets.

### Sprint 5 - Gold Layer

Create analytical credit risk datasets.

### Sprint 6 - Feature Engineering

Create model-ready features.

### Sprint 7 - PD Modeling

Train and evaluate a default prediction model.

### Sprint 8 - Expected Loss

Calculate PD, LGD, EAD, Expected Loss and PDD.

### Sprint 9 - Model Monitoring

Track PSI, score drift, AUC, KS, Gini and default rate.

## 13. Status

Current status: architecture definition phase.
