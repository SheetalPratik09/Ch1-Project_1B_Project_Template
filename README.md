# Ch1-Project_1B_Project_Template
# 🎵 Project: Data Modeling with Apache Cassandra — Sparkify

## 📌 Project Summary

Sparkify is a music streaming startup that collects user activity data in daily CSV files.
The analytics team wants to analyze **what songs users are listening to**, but the current data format does not support efficient queries.

In this project, an **ETL pipeline** is built in Python and **query-driven data models** are designed in **Apache Cassandra** to support fast analytical queries on song play data.

This project demonstrates:

* NoSQL data modeling using Cassandra
* ETL processing of raw CSV files
* Query-optimized table design
* Data loading and validation using SELECT queries

---

## 🎯 Business Questions Answered

The Cassandra tables are designed to answer the following queries:

1. **Songs played in a session**
   → What songs were played in a given session, and in what order?

2. **User and song by time in session**
   → Who listened to what song at what time in a session?

3. **Songs played by a user**
   → What songs has a specific user listened to?

Each query is supported by a **separate table**, following Cassandra best practices.

---

## 📂 Dataset Description

The dataset consists of multiple CSV files partitioned by date:

```
event_data/
│
├── 2018-11-08-events.csv
├── 2018-11-09-events.csv
├── ...
```

Each file contains user activity logs such as:

* userId
* firstName, lastName
* sessionId
* song, artist
* itemInSession
* timestamp
* page

Only records where:

```
page == "NextSong"
```

are used, since those represent actual song plays.

---

## 🔄 ETL Pipeline

### ✅ Step 1: Combine CSV Files

The ETL process:

```
Multiple CSV files
        │
        ▼
Read + Filter (NextSong only)
        │
        ▼
Select required columns
        │
        ▼
event_datafile_new.csv
```

This produces a **denormalized dataset** used for Cassandra inserts.

---

### ✅ Step 2: Load Data into Cassandra

Rows from `event_datafile_new.csv` are inserted into three Cassandra tables, each created for a specific query.

---

## 🧱 Data Modeling Strategy (Query-Driven)

Cassandra requires that:

* Tables are designed **based on query patterns**
* Partition keys must appear in WHERE clauses
* Joins are not supported

Therefore:

* Each query has its **own table**
* Data is **duplicated across tables** to optimize reads

---

## 🗝️ Table Designs

### ✅ Table 1 — Songs Played in Session

**Query:**
Get artist, song, and song length by session and item number.

**Table:** `songs_by_session`

**Primary Key:**

```
((session_id), item_in_session)
```

* Partition Key: `session_id`
* Clustering Key: `item_in_session` (keeps play order)

---

### ✅ Table 2 — User and Song by Time in Session

**Query:**
Get user, artist, and song by session and time.

**Table:** `users_by_session_time`

**Primary Key:**

```
((session_id), start_time)
```

* Partition Key: `session_id`
* Clustering Key: `start_time`

---

### ✅ Table 3 — Songs Played by User

**Query:**
Get all songs listened to by a user.

**Table:** `songs_by_user`

**Primary Key:**

```
((user_id), start_time)
```

* Partition Key: `user_id`
* Clustering Key: `start_time`

---

## ▶️ How to Run the Project

### ✅ Prerequisites

* Python 3.x
* Apache Cassandra running locally
* Cassandra Python driver installed

Install driver if needed:

```bash
pip install cassandra-driver
```

---

### ✅ Run Steps

1. Start Cassandra service
2. Open Jupyter Notebook

```bash
jupyter notebook
```

3. Open the project notebook
4. Run cells **top to bottom**:

   * ETL processing
   * Keyspace creation
   * Table creation
   * Data insertion
   * Query validation

Expected output:
Each SELECT query prints correct results matching the project questions.

---

## 🧪 Validation Strategy

After inserts, SELECT queries are executed using:

* Exact partition keys
* No `ALLOW FILTERING`

This ensures:

* Efficient lookups
* Proper Cassandra modeling

---

## 🧼 Code Quality Practices

* Modular notebook sections
* Clear markdown explanations per query
* Correct data types used in CREATE TABLE
* Tables dropped before recreation for clean testing
* Meaningful table names reflecting query intent

---

## ⚠️ Cassandra Concepts Applied

| Concept                 | Applied |
| ----------------------- | ------- |
| Query-first modeling    | ✅       |
| Denormalization         | ✅       |
| No joins                | ✅       |
| Composite primary keys  | ✅       |
| Partition-aware queries | ✅       |

---

## 🌟 Optional Enhancements

Possible improvements:

* Batch inserts for performance
* Logging instead of print statements
* Dockerized Cassandra setup
* Conversion to Python scripts

---

## 📚 What This Project Demonstrates

This project shows ability to:

* Design NoSQL schemas based on access patterns
* Build ETL pipelines using Python
* Apply Cassandra data modeling principles
* Translate business questions into database schemas

✅ **Short project explanation for interviews**
✅ **Architecture diagram (visual)**
✅ **Final rubric compliance checklist**

Just tell me what you’d like next, Sheetal 😊
