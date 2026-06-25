# DBMS II Mid Semester Examination Q&A (Summer 2023-2024)

> [!question] Question 1 a) ER to Relational Mapping (5 Marks)
> Analyze Figure 1 (an ER diagram showing Entity A, Entity B, and a Relation). Determine how many DDL statements are required to implement it, assuming mapping cardinality is not specified. Provide example codes to justify your answer.

> [!info]- Answer
> Based on standard ER to Relational mapping principles:
> If mapping cardinality is **not specified**, the safest and most robust approach is to assume a **Many-to-Many (M:N)** relationship. 
> A Many-to-Many relationship requires **3 DDL statements** (3 tables) to implement: one for Entity A, one for Entity B, and one for the relationship table connecting them.
> 
> *Example Code:*
> ```sql
> -- Table for Entity A
> CREATE TABLE EntityA (
>     A_ID INT PRIMARY KEY,
>     A_Attr VARCHAR(50)
> );
> 
> -- Table for Entity B
> CREATE TABLE EntityB (
>     B_ID INT PRIMARY KEY,
>     B_Attr VARCHAR(50)
> );
> 
> -- Table for the Relationship
> CREATE TABLE RelationAB (
>     A_ID INT,
>     B_ID INT,
>     PRIMARY KEY (A_ID, B_ID),
>     FOREIGN KEY (A_ID) REFERENCES EntityA(A_ID),
>     FOREIGN KEY (B_ID) REFERENCES EntityB(B_ID)
> );
> ```

> [!error] Question 1 b) ID Design (5 Marks) - *Out of Syllabus*
> Design and briefly explain a format for employee IDs intended for an automated Employee Salary Management system in a corporate office.

> [!info]- Answer
> A good Employee ID format should be unique, scalable, and provide basic context without exposing sensitive information.
> 
> **Proposed Format:** `DEPT-YYYY-NNNN`
> *   `DEPT`: A 2 to 4 letter department code (e.g., `HR`, `IT`, `FIN`).
> *   `YYYY`: The year the employee joined the company.
> *   `NNNN`: A 4-digit sequential number assigned uniquely within that year and department.
> 
> *Example:* `IT-2023-0042` represents the 42nd employee hired in the IT department in 2023. This format is easily readable, helps in grouping employees by department/seniority, and integrates seamlessly into an automated salary system.

> [!error] Question 1 c) Database Storage & DDLs (5 Marks) - *Out of Syllabus*
> * Explain if knowing the exact file location of database records is important.
> * Issue the DDLs for four tables (T1, T2, T3, T4).
> * **Constraint:** T1, T2, and T4 must be stored at `C:\MyData`, while T3 must be stored at `D:\MyData`. You may assume any attributes and datatypes.

> [!info]- Answer
> **Importance of Exact File Location:**
> For end-users and application developers, knowing the exact physical file location is **not important** due to *logical data independence*; the DBMS abstracts the physical storage details. However, for a Database Administrator (DBA), it is **crucial** for performance tuning (e.g., separating I/O heavy tables across different disks), backups, disaster recovery, and capacity planning.
> 
> **DDL Implementation:**
> To store tables in specific directory paths, we use `TABLESPACE` (syntax applicable to databases like PostgreSQL or Oracle).
> 
> ```sql
> -- Create Tablespaces mapping to physical directories
> CREATE TABLESPACE ts_c_drive LOCATION 'C:\MyData';
> CREATE TABLESPACE ts_d_drive LOCATION 'D:\MyData';
> 
> -- Create tables and assign them to the respective tablespaces
> CREATE TABLE T1 (id INT PRIMARY KEY, name VARCHAR(50)) TABLESPACE ts_c_drive;
> CREATE TABLE T2 (id INT PRIMARY KEY, description TEXT) TABLESPACE ts_c_drive;
> CREATE TABLE T3 (id INT PRIMARY KEY, data_value NUMERIC) TABLESPACE ts_d_drive;
> CREATE TABLE T4 (id INT PRIMARY KEY, created_at DATE) TABLESPACE ts_c_drive;
> ```

> [!error] Question 2 a) Large Objects (5 Marks) - *Out of Syllabus*
> Define "Large Object" and mention one specific application where it is used.

> [!info]- Answer
> **Definition:**
> A Large Object (LOB) is a specialized data type used in databases to store massive amounts of unstructured data that exceed standard field size limits (often up to several gigabytes). Common LOB types include BLOB (Binary Large Object) for binary data and CLOB (Character Large Object) for large text data.
> 
> **Specific Application:**
> A **Healthcare Management System** uses BLOBs to securely store large multimedia patient files, such as high-resolution MRI scans, X-rays, and ultrasound videos directly linked to a patient's medical record.

> [!question] Question 2 b) Memory Efficiency (5 Marks)
> Write PL/SQL code to demonstrate that `char` is more memory efficient than `varchar`.

> [!info]- Answer
> While `varchar` is generally more efficient for variable-length strings because it doesn't pad with spaces, `char` can be slightly more efficient for *fixed-length* strings. This is because `varchar` requires an extra byte or two to store the length prefix of the string, whereas `char` does not need length overhead (it's implicitly known).
> 
> *Example PL/SQL Demonstration:*
> ```sql
> DO $$ 
> DECLARE
>     v_char char(4) := 'A';       -- Stored as 'A   ' (4 bytes)
>     v_varchar varchar(4) := 'A'; -- Stored as 'A' + 1 byte length header (2 bytes here, but max 5)
>     
>     size_char INT;
>     size_varchar INT;
> BEGIN
>     -- Note: pg_column_size returns the actual storage size in bytes
>     size_char := pg_column_size(v_char);
>     size_varchar := pg_column_size(v_varchar);
>     
>     RAISE NOTICE 'Size of char(4) "A": % bytes', size_char;
>     RAISE NOTICE 'Size of varchar(4) "A": % bytes', size_varchar;
>     
>     -- For purely fixed strings (like exactly 4 chars), varchar adds a length header overhead byte.
> END; 
> $$;
> ```
> *(Note: In PostgreSQL, both `char` and `varchar` use the same underlying structure, so differences are mostly about padding vs length headers, but conceptually `char` saves the length-byte overhead for exact-length data).*

> [!question] Question 2 c) Code Correction (5 Marks)
> Identify and correct errors in the following PL/SQL function:
> ```sql
> create or replace function get_status (p_name IN varchar2 (20)) return number (6,2)
> IS
> v_name varchar2(20);
> BEGIN
> select name into v_name from students where
> name=p_name; p_name:='test';
> END;
> ```

> [!info]- Answer
> Since we are following **PostgreSQL**, this snippet contains several Oracle-specific syntax errors alongside general logic errors:
> 
> **Errors in the provided code:**
> 1. **Oracle specific types and keywords:** 
>    - `varchar2` is Oracle syntax. It should be `varchar`.
>    - `return number(6,2)` is Oracle syntax. It should be `RETURNS numeric(6,2)`. Notice it must be `RETURNS` instead of `return`.
>    - `IS` is Oracle syntax. In Postgres, we use `AS $$` before the body block.
> 2. **Size constraints on parameters:** Postgres does not allow length constraints on function parameters. `varchar(20)` should just be `varchar`.
> 3. **Modifying an IN parameter:** The parameter `p_name` is an `IN` parameter (which is the default in Postgres), making it read-only. `p_name:='test';` is invalid.
> 4. **Missing RETURN statement:** The function is declared to return a value, but the execution body lacks a `RETURN <value>;` statement.
> 5. **Missing Language Declaration:** Postgres requires the function body to be wrapped in quotes or `$$` and specify `LANGUAGE plpgsql`.
> 
> **Corrected Code (PostgreSQL):**
> ```sql
> CREATE OR REPLACE FUNCTION get_status(p_name IN varchar) 
> RETURNS numeric(6,2) AS $$
> DECLARE
>     v_name varchar(20);
>     v_status numeric(6,2) := 0.00; -- Example variable to hold return value
> BEGIN
>     SELECT name INTO v_name FROM students WHERE name = p_name;
>     
>     -- Removed assignment to read-only IN parameter p_name
>     
>     -- Ensure a value is returned
>     RETURN v_status; 
> END;
> $$ LANGUAGE plpgsql;
> ```

> [!question] Question 3 a) Table Creation (5 Marks)
> Create the necessary tables while maintaining standard database design principles.
> *Context:* Students are admitted to various programs within departments. Each course is defined by its code, title, credit hour, and type (Theory or Lab). The credit hour determines the total possible marks; for instance, a 3-credit course is graded out of 300 marks.

> [!info]- Answer
> Based on the context provided, we need to introduce tables for Departments and Programs to maintain standard 3NF normalization.
> 
> ```sql
> -- Departments
> CREATE TABLE Departments (
>     dept_id SERIAL PRIMARY KEY,
>     dept_name VARCHAR(100) NOT NULL UNIQUE
> );
> 
> -- Programs within Departments
> CREATE TABLE Programs (
>     program_id SERIAL PRIMARY KEY,
>     program_name VARCHAR(100) NOT NULL,
>     dept_id INT REFERENCES Departments(dept_id)
> );
> 
> -- Students admitted to Programs
> CREATE TABLE Students (
>     student_id INT PRIMARY KEY,
>     student_name VARCHAR(100) NOT NULL,
>     program_id INT REFERENCES Programs(program_id)
> );
> 
> -- Courses
> CREATE TABLE Courses (
>     course_id VARCHAR(10) PRIMARY KEY,
>     title VARCHAR(100) NOT NULL,
>     credit_hours NUMERIC(3,1) NOT NULL,
>     course_type VARCHAR(10) CHECK (course_type IN ('Theory', 'Lab'))
> );
> 
> -- Results (Associates Students and Courses with Marks)
> CREATE TABLE Results (
>     result_id SERIAL PRIMARY KEY,
>     student_id INT REFERENCES Students(student_id),
>     course_id VARCHAR(10) REFERENCES Courses(course_id),
>     total_marks_obtained NUMERIC(5,2) NOT NULL,
>     UNIQUE (student_id, course_id)
> );
> ```

> [!question] Question 3 b) Grade Calculation Function (7 Marks)
> Write a PL/SQL function that takes `Course ID` and `Total Marks Obtained` as input and returns a `Letter Grade` based on these thresholds:
> * **Theory:** A (80-100%), B (60-79%), C (50-59%), F (<50%).
> * **Lab:** A (80-100%), B (70-79%), C (60-69%), F (<60%).

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION get_letter_grade(p_course_id VARCHAR, p_marks_obtained NUMERIC)
> RETURNS VARCHAR AS $$
> DECLARE
>     v_course_type VARCHAR;
>     v_credit_hours NUMERIC;
>     v_total_marks NUMERIC;
>     v_percentage NUMERIC;
>     v_grade VARCHAR(1);
> BEGIN
>     -- Fetch course details
>     SELECT course_type, credit_hours INTO v_course_type, v_credit_hours
>     FROM Courses
>     WHERE course_id = p_course_id;
>     
>     IF NOT FOUND THEN
>         RAISE EXCEPTION 'Course not found';
>     END IF;
>     
>     -- Calculate percentage (Assuming 1 credit = 100 marks based on the prompt)
>     v_total_marks := v_credit_hours * 100;
>     v_percentage := (p_marks_obtained / v_total_marks) * 100;
>     
>     -- Determine Grade based on type
>     IF v_course_type = 'Theory' THEN
>         IF v_percentage >= 80 THEN v_grade := 'A';
>         ELSIF v_percentage >= 60 THEN v_grade := 'B';
>         ELSIF v_percentage >= 50 THEN v_grade := 'C';
>         ELSE v_grade := 'F';
>         END IF;
>     ELSIF v_course_type = 'Lab' THEN
>         IF v_percentage >= 80 THEN v_grade := 'A';
>         ELSIF v_percentage >= 70 THEN v_grade := 'B';
>         ELSIF v_percentage >= 60 THEN v_grade := 'C';
>         ELSE v_grade := 'F';
>         END IF;
>     END IF;
>     
>     RETURN v_grade;
> END;
> $$ LANGUAGE plpgsql;
> ```

> [!question] Question 3 c) CGPA Calculation Function (8 Marks)
> Write a PL/SQL function that takes a `Student ID` as input and outputs the `CGPA Obtained`.
> * **Grading Values:** A=4.00, B=3.50, C=3.00, F=0.00.
> * Use the standard formula (Sum of [Grade Value × Credit Hours] / Total Credit Hours).

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION calculate_cgpa(p_student_id INT)
> RETURNS NUMERIC(3,2) AS $$
> DECLARE
>     v_total_points NUMERIC := 0;
>     v_total_credits NUMERIC := 0;
>     v_cgpa NUMERIC(3,2);
>     v_record RECORD;
>     v_letter_grade VARCHAR(1);
>     v_grade_value NUMERIC;
> BEGIN
>     -- Loop through all results for the student
>     FOR v_record IN (
>         SELECT r.course_id, r.total_marks_obtained, c.credit_hours 
>         FROM Results r
>         JOIN Courses c ON r.course_id = c.course_id
>         WHERE r.student_id = p_student_id
>     ) LOOP
>         -- Get letter grade using the previous function
>         v_letter_grade := get_letter_grade(v_record.course_id, v_record.total_marks_obtained);
>         
>         -- Map letter to grade value
>         CASE v_letter_grade
>             WHEN 'A' THEN v_grade_value := 4.00;
>             WHEN 'B' THEN v_grade_value := 3.50;
>             WHEN 'C' THEN v_grade_value := 3.00;
>             WHEN 'F' THEN v_grade_value := 0.00;
>             ELSE v_grade_value := 0.00;
>         END CASE;
>         
>         -- Accumulate totals
>         v_total_points := v_total_points + (v_grade_value * v_record.credit_hours);
>         v_total_credits := v_total_credits + v_record.credit_hours;
>     END LOOP;
>     
>     -- Calculate CGPA
>     IF v_total_credits > 0 THEN
>         v_cgpa := v_total_points / v_total_credits;
>     ELSE
>         v_cgpa := 0.00;
>     END IF;
>     
>     RETURN v_cgpa;
> END;
> $$ LANGUAGE plpgsql;
> ```
