# DBMS II Mid Semester Exam - Study Guide

Based on an analysis of past mid-semester exams (2018-2023), this guide outlines the exam structure, typical question formats, and key areas you must focus on to ace the test.

## 1. Exam Structure & Question Types
The exam typically consists of 3-4 major questions (50 Marks total, 1.5 Hours). The questions evaluate four main areas:
- **Conceptual & Theoretical Knowledge** (10-15 marks)
- **Database Design (ER & DDL)** (10-15 marks)
- **SQL Queries** (5-10 marks)
- **PL/pgSQL Programming (Functions, Procedures, Triggers)** (20-30 marks)

## 2. Key Topics to Focus On

### A. Conceptual Knowledge
* **Datatypes & Memory:** Be able to explain the memory efficiency of `char` vs. `varchar` (fixed vs. variable length, length headers).
* **PL/SQL Features:** Know the distinct advantages of using `%TYPE` (dynamic datatype adaptation) and `%ROWTYPE`.
* **Functions vs. Procedures:** Understand the core differences, specifically that procedures support transaction control (`COMMIT`/`ROLLBACK`), whereas functions run within a single transaction.
* **Keys & Cardinality:** Know how to implement Many-to-Many cardinality using junction tables, the guidelines for selecting primary keys, and the purpose of foreign keys.

### B. Database Design & DDLs
* **Scenario-Based DDLs:** Almost every year features a scenario (e.g., Telecom system, Result Processing, Student Allowance) where you must write `CREATE TABLE` statements.
* **Focus Areas:** Memorize how to define `PRIMARY KEY`, `FOREIGN KEY` (with `REFERENCES`), `CHECK` constraints, `UNIQUE` constraints, and standard datatypes (`SERIAL`, `NUMERIC`, `VARCHAR`).

### C. Standard SQL Queries
* **Query Writing:** You may be asked to write `SELECT` queries utilizing `JOIN`s, `GROUP BY`, and aggregate functions (`COUNT`, `AVG`).
* **Categorization:** Using `CASE` statements inside a `SELECT` clause to classify data (e.g., High/Medium/Low salary).
* **Updates:** Writing `UPDATE` queries combined with subqueries.

### D. PL/pgSQL Programming (The Core of the Exam)
This is the most heavily weighted section. Focus on:
1. **Writing Functions:** Know the exact syntax: `CREATE OR REPLACE FUNCTION name(args) RETURNS type AS $$ ... $$ LANGUAGE plpgsql;`
2. **Writing Procedures:** Using implicit cursors to loop through data: `FOR record_var IN (SELECT ...) LOOP`.
3. **Business Logic Implementation:** Be prepared to write logic that calculates thresholds, percentages, grades, total bills, or categorizes customers based on data in tables.
4. **Triggers:** Know how to write a trigger function (`RETURNS TRIGGER`, returning `NEW` or `OLD`) and how to bind it (`CREATE TRIGGER ... BEFORE/AFTER INSERT ON ...`).
5. **Code Correction:** Identifying syntax errors in Oracle PL/SQL snippets and converting them to PostgreSQL PL/pgSQL (e.g., removing length limits like `VARCHAR(20)` in parameters, using `$$`, using `RETURNS` instead of `return`).

---

## 3. Gap Analysis: Do Your Notes Cover This?

I have cross-referenced these exam requirements with your current notes (`DBMS Mid C.md` and `Mid Slides (Summary).md`). 

### ✅ Well-Covered in Notes
* **PL/pgSQL Basics:** Syntax, variables, blocks, and control structures (IF, CASE, LOOP) are extensively covered.
* **Datatypes:** Deep dives into numeric, arrays, ranges, network IPs, and composites.
* **Functions, Procedures & Cursors:** Excellent coverage of `%TYPE`, parameters, and looping techniques.
* **Triggers:** Full chapter with syntax, execution flow (`NEW`/`OLD`), and multiple examples.
* **Exception Handling:** Covered thoroughly.

### ⚠️ Missing or Briefly Touched Upon (What you need to review elsewhere)
* **ER Diagram to Relational Mapping:** The notes list "Mapping Cardinality" in the syllabus overview, but they lack explicit examples on how to convert ER diagrams (like Many-to-Many relationships) into junction tables.
* **Complex DDL Scenarios:** The notes assume you already remember how to write complex `CREATE TABLE` statements with foreign keys, checks, and constraints from DBMS I. You should practice these manually.
* **Standard SQL Practice:** Complex `JOIN`s, `GROUP BY` aggregates, and nested subqueries are sparse in the current notes. You will need to refresh your DBMS I SQL knowledge.
* **Tablespaces & Data Warehousing:** Asked in older exams (2018/2022) but completely missing from your current notes. If your instructor explicitly removed these from the syllabus, you are fine; otherwise, you must study them.
* **Traditional File Processing vs. DBMS:** Asked in 2018, not covered in the current material.
