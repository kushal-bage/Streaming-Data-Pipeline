# Realtime Data Streaming | End-to-End Data Engineering Project

## Table of Contents
- [Introduction](#introduction)
- [System Architecture](#system-architecture)
- [What You'll Learn](#what-youll-learn)
- [Technologies](#technologies)
- [Getting Started](#getting-started)
- [Watch the Video Tutorial](#watch-the-video-tutorial)

## Process
 ┌─────────────┐       ┌───────────────┐       ┌───────────────┐       ┌───────────────┐
 │  Data Source│       │   Airflow DAG │       │ Spark Structured│       │   Cassandra   │
 │ (API/DB/...)│       │   Scheduler   │       │   Streaming     │       │   Keyspace    │
 └──────┬──────┘       └──────┬────────┘       └──────┬────────  ┘       └──────┬────────┘
        │                     │                      │                        │
        │   1. Extract        │                      │                        │
        ├────────────────────▶│                      │                        │
        │                     │                      │                        │
        │                     │  2. Trigger Spark   │                        │
        │                     ├────────────────────▶│                        │
        │                     │                      │                        │
        │                     │                      │  3. Connect to Kafka   │
        │                     │                      ├──────────────────────▶│
        │                     │                      │                        │
        │                     │                      │  4. Micro-batch read   │
        │                     │                      │    (e.g., 10 sec)     │
        │                     │                      │                        │
        │                     │                      │  5. Transform / Format │
        │                     │                      │                        │
        │                     │                      │  6. Write to Cassandra │
        │                     │                      └──────────────────────▶│
        │                     │                      │                        │
        │                     │                      │  7. Optionally TTL /   │
        │                     │                      │     Partition / Archive│
        │                     │                      │                        │


### อธิบาย flow

1.Data Source → Airflow DAG

- Airflow รับข้อมูลแบบ batch หรือ schedule DAG เพื่อ trigger Spark job

- DAG ไม่จำเป็นต้องรันทุกวินาที; อาจ run ทุก 1 นาที หรือชั่วโมงก็ได้

2.Airflow → Spark Streaming

- DAG trigger Spark Streaming job

- Spark จะเชื่อม Kafka และอ่านข้อมูลแบบ micro-batch (เช่น ทุก 10 วินาที)

3.Spark Streaming → Kafka

- Spark อ่าน data ใหม่จาก Kafka topic

4.Spark Streaming → Transform → Cassandra

- Spark transform ข้อมูลแล้วเขียนเข้า Cassandra

- สามารถกำหนด checkpoint เพื่อ recover job ถ้า crash

- สามารถกำหนด TTL / partition เพื่อลดขนาด database

## Introduction

This project serves as a comprehensive guide to building an end-to-end data engineering pipeline. It covers each stage from data ingestion to processing and finally to storage, utilizing a robust tech stack that includes Apache Airflow, Python, Apache Kafka, Apache Zookeeper, Apache Spark, and Cassandra. Everything is containerized using Docker for ease of deployment and scalability.

## System Architecture

![System Architecture](https://github.com/airscholar/e2e-data-engineering/blob/main/Data%20engineering%20architecture.png)

The project is designed with the following components:

- **Data Source**: We use `randomuser.me` API to generate random user data for our pipeline.
- **Apache Airflow**: Responsible for orchestrating the pipeline and storing fetched data in a PostgreSQL database.
- **Apache Kafka and Zookeeper**: Used for streaming data from PostgreSQL to the processing engine.
- **Control Center and Schema Registry**: Helps in monitoring and schema management of our Kafka streams.
- **Apache Spark**: For data processing with its master and worker nodes.
- **Cassandra**: Where the processed data will be stored.

## What You'll Learn

- Setting up a data pipeline with Apache Airflow
- Real-time data streaming with Apache Kafka
- Distributed synchronization with Apache Zookeeper
- Data processing techniques with Apache Spark
- Data storage solutions with Cassandra and PostgreSQL
- Containerizing your entire data engineering setup with Docker

## Technologies

- Apache Airflow
- Python
- Apache Kafka
- Apache Zookeeper
- Apache Spark
- Cassandra
- PostgreSQL
- Docker

## Getting Started

1. Clone the repository:
    ```bash
    git clone https://github.com/airscholar/e2e-data-engineering.git
    ```

2. Navigate to the project directory:
    ```bash
    cd e2e-data-engineering
    ```

3. Run Docker Compose to spin up the services:
    ```bash
    docker-compose up
    ```

For more detailed instructions, please check out the video tutorial linked below.

## Watch the Video Tutorial

For a complete walkthrough and practical demonstration, check out our [YouTube Video Tutorial](https://www.youtube.com/watch?v=GqAcTrqKcrY).
