---
title: DBMS II Final Semester Examination Q&A (Summer 2023-2024)
date: 2026-09-02
tags:
  - dbms
  - exam
  - final
  - plpgsql
  - nosql
  - data-warehouse
  - graph-db
aliases:
  - Final Summer 2023-2024 QnA
---

# DBMS II Final Semester Examination Q&A (Summer 2023-2024)

> [!note] Syllabus & Dialect Notice
> All procedural and relational database solutions are written strictly in **PostgreSQL / PL/pgSQL**, and graph database solutions are in **Cypher (Neo4j)**.

---

## Question 1

> [!question] Question 1 a) Foreign Key Redundancy & Inconsistency (5 Marks)
> Foreign key removes redundancy and inconsistency. Explain with suitable example.

> [!info]- Answer
> ### 1. Theoretical Concept
> A **Foreign Key (FK)** enforces referential integrity between two tables by establishing a parent-child relationship. It eliminates two critical data anomalies:
> * **Redundancy:** Instead of repeating non-key descriptive attributes across multiple related records, the data is normalized into a parent table, and only the foreign key identifier is stored in dependent tables.
> * **Inconsistency:** Prevents data divergence and orphaned records. If parent data updates or deletes occur, foreign key constraints (such as `ON UPDATE CASCADE` or `ON DELETE RESTRICT`) ensure uniform, synchronized state across the entire database.
> 
> ### 2. Demonstration (PostgreSQL)
> 
> #### ❌ Without Foreign Key (Redundant & Inconsistent)
> ```sql
> -- Storing department details directly in employee rows
> CREATE TABLE bad_employees (
>     emp_id INT PRIMARY KEY,
>     emp_name VARCHAR(50),
>     dept_name VARCHAR(50),      -- Redundant: Repeated for every employee in the department
>     dept_building VARCHAR(50)   -- Inconsistent: Modifying building for one row leaves others inconsistent!
> );
> ```
> 
> #### ✅ With Foreign Key (Normalized & Consistent)
> ```sql
> -- Parent Table: Department details stored in a single place
> CREATE TABLE departments (
>     dept_id INT PRIMARY KEY,
>     dept_name VARCHAR(50) NOT NULL,
>     building VARCHAR(50) NOT NULL
> );
> 
> -- Child Table: References the parent department via FK
> CREATE TABLE employees (
>     emp_id INT PRIMARY KEY,
>     emp_name VARCHAR(50) NOT NULL,
>     dept_id INT NOT NULL,
>     CONSTRAINT fk_emp_dept FOREIGN KEY (dept_id) 
>         REFERENCES departments(dept_id)
>         ON UPDATE CASCADE
>         ON DELETE RESTRICT
> );
> ```
> * **Redundancy Removed:** Department names and buildings are stored once in `departments`.
> * **Inconsistency Prevented:** Attempting to assign an employee to a non-existent `dept_id` is immediately blocked.

---

> [!question] Question 1 b) Item Popularity Function (10 Marks)
> Consider the following two table definitions for a typical super shop:
> ```sql
> create table items (
>     itemid number primary key,
>     itemname varchar2(20)
> );
> create table transactions (
>     tid number primary key,
>     itemid number,
>     no_of_item number,
>     total_price number,
>     sale_date date,
>     constraint fk_item foreign key(itemid) references items
> );
> ```
> Your task is to write a function in PL/pgSQL as follows:
> * **Input:** `itemid`
> * **Outputs:** `Popularity of the item`, `total number sold`
> * **Algorithm:** The item's popularity is `'A'` if the total number of sales for the item between **January 1, 2024** and **June 30, 2025** exceeds `1000`. Otherwise item's popularity is `'B'`.
> * *Note:* `no_of_item` in `transactions` table indicates total number for the item for that specific transaction only.

> [!info]- Answer
> In **PostgreSQL**, multiple return values are implemented using **`OUT` parameters** (or `RETURNS TABLE`).
> 
> ```sql
> CREATE OR REPLACE FUNCTION get_item_popularity(
>     IN  p_itemid           INT,
>     OUT p_popularity       CHAR(1),
>     OUT p_total_sold       NUMERIC
> )
> AS $$
> BEGIN
>     -- Calculate aggregate sales within the specified date window
>     SELECT COALESCE(SUM(no_of_item), 0)
>     INTO p_total_sold
>     FROM transactions
>     WHERE itemid = p_itemid
>       AND sale_date BETWEEN '2024-01-01' AND '2025-06-30';
> 
>     -- Popularity rating logic
>     IF p_total_sold > 1000 THEN
>         p_popularity := 'A';
>     ELSE
>         p_popularity := 'B';
>     END IF;
> END;
> $$ LANGUAGE plpgsql;
> ```
> 
> *Invocation Example:*
> ```sql
> SELECT * FROM get_item_popularity(101);
> -- Returns: (p_popularity, p_total_sold)
> ```

---

> [!question] Question 1 c) Payroll Bonus Automation (15 Marks)
> Consider the following scenario:
> In a Payroll Management System of a company a new rule has been approved. The company will award additional bonus based on the current salary. The rules of determination of the bonus amount is as follows:
> 
> | Condition | Bonus Amount |
> | :--- | :--- |
> | Monthly Salary above 90,000 | 3% of the salary |
> | Monthly Salary between 50,000 and 90,000 | 6% of the salary |
> | Monthly Salary below 50,000 | 15% of the salary |
> 
> *Note:* The bonus amount is not added to the original salary of the employees, rather a separate transaction (i.e. amount is determined by the business logic already stated) is made against each employee.
> 
> **Tasks:**
> 1. Create necessary DDLs to reflect the above requirement.
> 2. Write a PL/pgSQL procedure to automate the bonus process for all employees.

> [!info]- Answer
> ### 1. DDL Definitions
> ```sql
> -- Employees Table
> CREATE TABLE employees (
>     emp_id SERIAL PRIMARY KEY,
>     emp_name VARCHAR(100) NOT NULL,
>     salary NUMERIC(10, 2) NOT NULL CHECK (salary > 0)
> );
> 
> -- Separate Bonus Transactions Table
> CREATE TABLE bonus_transactions (
>     bonus_id SERIAL PRIMARY KEY,
>     emp_id INT NOT NULL REFERENCES employees(emp_id) ON DELETE CASCADE,
>     bonus_amount NUMERIC(10, 2) NOT NULL,
>     disbursement_date DATE DEFAULT CURRENT_DATE
> );
> ```
> 
> ### 2. PL/pgSQL Stored Procedure
> Using a **Cursor FOR Loop** to iterate through all active employees, compute the percentage based on the salary brackets, and log each bonus transaction:
> 
> ```sql
> CREATE OR REPLACE PROCEDURE process_employee_bonuses()
> LANGUAGE plpgsql
> AS $$
> DECLARE
>     v_emp RECORD;
>     v_bonus NUMERIC(10, 2);
> BEGIN
>     -- Iterate through each employee record
>     FOR v_emp IN SELECT emp_id, salary FROM employees LOOP
>         -- Evaluate bonus criteria
>         IF v_emp.salary > 90000 THEN
>             v_bonus := v_emp.salary * 0.03;
>         ELSIF v_emp.salary >= 50000 THEN
>             v_bonus := v_emp.salary * 0.06;
>         ELSE
>             v_bonus := v_emp.salary * 0.15;
>         END IF;
> 
>         -- Record individual bonus transaction
>         INSERT INTO bonus_transactions (emp_id, bonus_amount, disbursement_date)
>         VALUES (v_emp.emp_id, v_bonus, CURRENT_DATE);
>     END LOOP;
>     
>     RAISE NOTICE 'All bonus transactions have been processed successfully.';
> END;
> $$;
> ```
> 
> *Invocation:*
> ```sql
> CALL process_employee_bonuses();
> ```

---

## Question 2

> [!question] Question 2 a) Preventing Concurrency Anomaly during Bonus Processing (5 Marks)
> In question 1.c it may happen that while running this process some employees may be given additional increment (which is added to the salary) as a result of the salary update which may cause incorrect salary calculation. Propose a suitable solution to prevent this anomaly (write only the essential code).

> [!info]- Answer
> ### 1. Anomaly Cause & Solution
> * **The Cause:** A **Concurrency Anomaly (Race Condition / Non-Repeatable Read)** occurs when a concurrent update transaction modifies an employee's salary while the bonus procedure is in mid-execution, causing inconsistent bonus calculations.
> * **The Solution:** Apply **Pessimistic Row-Level Locking** by adding the **`FOR UPDATE`** clause to the cursor query. This locks the selected employee rows for the duration of the transaction, blocking concurrent salary increments until the bonus transaction finishes and commits.
> 
> ### 2. Essential Code (PL/pgSQL)
> 
> ```sql
> DECLARE
>     -- Declare cursor with row-level locking
>     cur_emp CURSOR FOR 
>         SELECT emp_id, salary 
>         FROM employees 
>         FOR UPDATE; -- Prevents concurrent salary increments during calculation
> ```
> 
> *(Alternative: Execute the procedure under the `REPEATABLE READ` or `SERIALIZABLE` transaction isolation level: `SET TRANSACTION ISOLATION LEVEL REPEATABLE READ;`)*

---

> [!question] Question 2 b) Schema Trigger vs. Database Trigger (5 Marks)
> State the basic difference between schema trigger and database trigger.

> [!info]- Answer
> | Feature / Aspect | **Schema Trigger (Object/Table Level)** | **Database Trigger (System/Event Level)** |
> | :--- | :--- | :--- |
> | **Scope & Target** | Attached to a **specific schema object** (a single table or view). | Attached to the **entire database instance** globally. |
> | **Firing Events** | Fires on DML operations (`INSERT`, `UPDATE`, `DELETE`) or object-level DDL on that table. | Fires on global system lifecycle events (`STARTUP`, `SHUTDOWN`, `SERVERERROR`, `LOGON`, `LOGOFF`) or database-wide DDL (`ddl_command_end`). |
> | **Primary Purpose** | Enforcing business logic, table-level auditing, data derivation, and foreign key cascades. | Database-wide security enforcement, global change tracking, preventing schema tampering, and monitoring user connections. |
> | **PostgreSQL Syntax** | `CREATE TRIGGER ... ON table_name` | `CREATE EVENT TRIGGER ... ON event_name` |

---

> [!question] Question 2 c) Item Borrowing History Archive Solution (10 Marks)
> Consider the following transaction table in a resource management system for a department where employees can borrow one item and return in time:
> ```sql
> create table transaction (
>     tid number primary key,
>     emp_id number, --- employee ID (fk not shown here)
>     item_id number, --- item ID (fk not shown here)
>     issue_date date,
>     return_date date
> );
> ```
> Notice that whenever any employee returns any borrowed item the entire record is not very useful in most cases. Hence database admin removes those records (with return value not null). But it creates another problem: the system is unable to produce any report containing the history of borrowed items for a specific employee.
> 
> How will you solve this problem? Explain each step clearly. Codes should be in PL/pgSQL.

> [!info]- Answer
> ### Architectural Solution: Historical Archiving via Triggers
> To prevent permanent loss of historical data upon deletion, we implement an **Archive Table (`transaction_history`)** and attach a **`BEFORE DELETE` Trigger** to automatically archive completed transactions before they are purged from the active table.
> 
> ### Step-by-Step Implementation:
> 
> #### Step 1: Create the History Archive Table
> ```sql
> CREATE TABLE transaction_history (
>     history_id SERIAL PRIMARY KEY,
>     tid INT NOT NULL,
>     emp_id INT NOT NULL,
>     item_id INT NOT NULL,
>     issue_date DATE NOT NULL,
>     return_date DATE NOT NULL,
>     archived_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
> );
> ```
> 
> #### Step 2: Create the Archive Trigger Function
> ```sql
> CREATE OR REPLACE FUNCTION archive_returned_transaction()
> RETURNS TRIGGER AS $$
> BEGIN
>     -- Only archive records where the item has actually been returned
>     IF OLD.return_date IS NOT NULL THEN
>         INSERT INTO transaction_history (tid, emp_id, item_id, issue_date, return_date)
>         VALUES (OLD.tid, OLD.emp_id, OLD.item_id, OLD.issue_date, OLD.return_date);
>     END IF;
>     RETURN OLD; -- Proceed with deletion from active table
> END;
> $$ LANGUAGE plpgsql;
> ```
> 
> #### Step 3: Attach the Trigger to the Active Transaction Table
> ```sql
> CREATE TRIGGER trg_archive_transaction
> BEFORE DELETE ON transaction
> FOR EACH ROW
> EXECUTE FUNCTION archive_returned_transaction();
> ```
> 
> #### Step 4: Report Generation
> Historical reports can now be generated from `transaction_history` without cluttering the active `transaction` table:
> ```sql
> SELECT tid, item_id, issue_date, return_date 
> FROM transaction_history 
> WHERE emp_id = 101 
> ORDER BY issue_date DESC;
> ```

---

## Question 3

> [!question] Question 3 a) Data Warehouse Separation & OLAP Operations (5 + 5 = 10 Marks)
> 1. What are the basic advantages of separating data warehouse from its operational database?
> 2. Differentiate between roll-up and drill-down operation used for data warehouse technology.

> [!info]- Answer
> ### 1. Advantages of Separating Data Warehouse (OLAP) from Operational DB (OLTP)
> 1. **Resource & Contention Isolation:** Operational systems handle concurrent, low-latency OLTP transactions. Heavy analytical aggregations running directly on OLTP would lock tables and degrade transaction throughput.
> 2. **Optimized Schema Architecture:** OLTP uses normalized schemas (3NF) to eliminate update anomalies. Data Warehouses use denormalized multidimensional schemas (Star / Snowflake) optimized for fast scan and aggregation queries.
> 3. **Historical Data Preservation:** OLTP stores current operational state; Data Warehouses retain multi-year time-series snapshots for trend analysis.
> 4. **Data Integration & Cleansing:** Data Warehouses consolidate and clean heterogeneous data from multiple disparate production databases via ETL.
> 
> ### 2. Roll-Up vs. Drill-Down Operations
> 
> | Feature | **Roll-Up (Drill-Up / Aggregation)** | **Drill-Down (Disaggregation)** |
> | :--- | :--- | :--- |
> | **Concept Direction** | Climbs **UP** the concept hierarchy (Generalization). | Steps **DOWN** the concept hierarchy (Specialization). |
> | **Data Granularity** | Decreases detail (coarser granularity). | Increases detail (finer granularity). |
> | **Dimension Handling** | Aggregates and collapses dimensions. | Adds new dimensions or splits existing ones into finer components. |
> | **Example** | Aggregating sales from `[Day] -> [Month] -> [Quarter] -> [Year]`. | Drilling into sales from `[Year] -> [Quarter] -> [Month] -> [Day]`. |

---

> [!question] Question 3 b) Performance of a Data System & Large Online System Measurement (10 Marks)
> Explain the concept of performance of a data system. How do you measure the performance of a large online system? Explain.

> [!info]- Answer
> ### 1. Concept of Data System Performance
> Performance of a data system measures its efficiency, capacity, and reliability under defined workloads. It consists of two fundamental pillars:
> * **Throughput:** The volume of work completed per unit time (e.g., Queries Per Second - QPS, Transactions Per Second - TPS).
> * **Response Time / Latency:** The elapsed time between sending a request and receiving the full response (service time + queueing delay + network latency).
> 
> ### 2. Measuring Performance in Large Online Systems
> Averages are misleading in distributed web-scale systems because tail latencies dominate the user experience. Measurement relies on:
> 
> 1. **Response Time Percentiles (Tail Latencies):**
>    * **Median ($p50$):** Half of user requests are faster, half slower.
>    * **High Percentiles ($p95$, $p99$, $p99.9$):** The latency experienced by the slowest 5%, 1%, or 0.1% of requests. Crucial because the most active/valuable users typically accumulate the highest data load and trigger multiple backend fan-out calls.
> 2. **Workload & Load Parameters:**
>    * Quantifying request distribution (e.g., read-to-write ratios, cache hit rates, concurrent active sessions).
> 3. **Service Level Agreements (SLAs) & SLOs:**
>    * Defining performance contracts (e.g., *"99.9% of requests must return within 200ms"*).
> 4. **Scalability Ratios:**
>    * Evaluating whether adding compute/storage capacity linearly restores performance under increased load.

---

> [!question] Question 3 c) Two-Phase Commit (2PC) in RDBMS vs. NoSQL (5 Marks)
> Explain how two-phase commit protocol ensures data availability in a relational database system. Why is this protocol not suitable for NoSQL applications?

> [!info]- Answer
> ### 1. Two-Phase Commit (2PC) in Relational Databases
> 2PC guarantees **Atomic Consistency across distributed databases** using a coordinator and two distinct phases:
> 1. **Phase 1 (Prepare):** The coordinator asks all participant nodes if they can commit. Nodes write to their undo/redo logs and reply `YES` (ready) or `NO`.
> 2. **Phase 2 (Commit/Abort):** If *all* nodes vote `YES`, the coordinator issues `COMMIT`; otherwise, it issues `ABORT` (rollback).
> 
> This ensures strict atomic durability (either all nodes commit or none do).
> 
> ### 2. Why 2PC is Unsuitable for NoSQL Applications
> 1. **Blocking Protocol (Violates High Availability):** If the coordinator crashes during Phase 2 after nodes vote `YES`, participant nodes remain locked and blocked indefinitely.
> 2. **High Latency Overhead:** Requires multiple synchronous network round-trips across nodes, degrading throughput in large clusters.
> 3. **CAP Theorem Incompatibility:** NoSQL architectures prioritize **Partition Tolerance and Availability (AP)** or horizontal scalability, adopting **Eventual Consistency / BASE** and lightweight consensus algorithms (e.g., Raft/Paxos) instead of synchronous blocking 2PC.

---

## Question 4

> [!question] Question 4 a) Concept of "SQL Strain" in Modern Scalable Applications (10 Marks)
> Explain the concept of "SQL Strain" in the context of performance of a modern scalable application.

> [!info]- Answer
> ### 1. Definition
> **SQL Strain** describes the severe performance breakdown, high latency, and architectural bottlenecks that relational databases (RDBMS) suffer when handling highly connected, recursive, or dynamic web-scale data models.
> 
> ### 2. Causes of SQL Strain
> 
> ```
> Relational RDBMS: Join-table Explosion O(k^d) vs. Graph DB: Index-Free Adjacency O(1)
> ```
> 
> 1. **Exponential Join Cost on Deep Traversals:**
>    * Querying interconnected relationships (social networks, recommendation engines, fraud detection) requires recursive multiple `JOIN` operations across relational tables.
>    * Query complexity grows exponentially with traversal depth ($O(k^d)$ where $k$ is the branching factor and $d$ is the depth), exhausting memory and CPU.
> 2. **Impedance Mismatch:**
>    * Object graphs and nested data models in modern applications map awkwardly to flat relational tables, requiring complex ORM mapping overhead.
> 3. **Schema Rigidity:**
>    * Rigid relational DDL makes continuous schema evolution difficult, leading to sparse tables with numerous `NULL` columns.
> 4. **Scale-Up vs. Scale-Out Limits:**
>    * Traditional RDBMS relies on centralized ACID locking and does not partition graph-like connected datasets smoothly across commodity clusters.

---

> [!question] Question 4 b) Graph DB Network Construction & Cypher Queries (6 + 9 = 15 Marks)
> Use Graph SQL (Cypher) to create a network of different types of nodes (i.e. `Person` and `Book`) and relations as shown in Figure 1.
> 
> *Note:* Nodes with visual label `'C Basic'` and `'C++'` are `Book`s while other nodes are `Person`s (`X`, `B`, `E`, `D`). The relations are labeled with each edge:
> * `(X)-[:writer]->('C Basic')`
> * `(B)-[:writer]->('C Basic')`
> * `(B)-[:writer]->('C++')`
> * `(B)-[:friend]->(E)`
> * `(B)-[:friend]->(D)`
> * `('C Basic')-[:prerequisite]->('C++')`
> 
> Now write Graph SQL in **Cypher** for the following:
> 1. Show the entire graph with all nodes and relations as shown in Figure 1.
> 2. Show the subgraph only for the relation `friend`.
> 3. Show the subgraph for the writer(s) of the book `'C Basic'`.

> [!info]- Answer
> ### 1. Graph Construction (Cypher DDL / DML)
> ```cypher
> // Create Person Nodes
> CREATE (x:Person {name: 'X'})
> CREATE (b:Person {name: 'B'})
> CREATE (e:Person {name: 'E'})
> CREATE (d:Person {name: 'D'})
> 
> // Create Book Nodes
> CREATE (c_basic:Book {title: 'C Basic'})
> CREATE (cpp:Book {title: 'C++'})
> 
> // Create Relationships
> CREATE (x)-[:writer]->(c_basic)
> CREATE (b)-[:writer]->(c_basic)
> CREATE (b)-[:writer]->(cpp)
> CREATE (b)-[:friend]->(e)
> CREATE (b)-[:friend]->(d)
> CREATE (c_basic)-[:prerequisite]->(cpp);
> ```
> 
> ---
> 
> ### 2. Cypher Queries
> 
> #### i) Show the entire graph with all nodes and relations
> ```cypher
> MATCH (n)-[r]->(m)
> RETURN n, r, m;
> ```
> 
> #### ii) Show the subgraph only for the relation `friend`
> *Visual Output:* `(B)-[:friend]->(E)` and `(B)-[:friend]->(D)`
> ```cypher
> MATCH (p1:Person)-[r:friend]->(p2:Person)
> RETURN p1, r, p2;
> ```
> 
> #### iii) Show the subgraph for the writer(s) of the book `'C Basic'`
> *Visual Output:* `(X)-[:writer]->(C Basic)` and `(B)-[:writer]->(C Basic)`
> ```cypher
> MATCH (p:Person)-[r:writer]->(b:Book {title: 'C Basic'})
> RETURN p, r, b;
> ```

---

## Related Notes & References
* [[PL-pgSQL, Triggers & Graph Databases]] — Stored procedures, cursors, triggers, locking, and Neo4j Cypher.
* [[Data Warehouse Technology]] — OLTP vs OLAP, ETL, Roll-up & Drill-down multidimensional operations.
* [[NoSQL Databases]] — Web-scale distributed systems, Tail Latency percentiles, 2PC, and SQL Strain.
