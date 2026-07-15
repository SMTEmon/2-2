# DBMS II Mid Semester Examination Q&A (All Years)

## Summer Semester 2023-2024

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
> Create the necessary tables following standard database design principles for a Result Processing System where courses have codes, titles, credit hours (e.g., 3 credits = 300 marks), and types (Theory or Lab).

> [!info]- Answer
> ```sql
> -- Table for Students
> CREATE TABLE Students (
>     student_id INT PRIMARY KEY,
>     student_name VARCHAR(100) NOT NULL
> );
> 
> -- Table for Courses
> CREATE TABLE Courses (
>     course_id VARCHAR(10) PRIMARY KEY,
>     title VARCHAR(100) NOT NULL,
>     credit_hours NUMERIC(3,1) NOT NULL,
>     course_type VARCHAR(10) CHECK (course_type IN ('Theory', 'Lab'))
> );
> 
> -- Table for Results
> CREATE TABLE Results (
>     result_id SERIAL PRIMARY KEY,
>     student_id INT REFERENCES Students(student_id),
>     course_id VARCHAR(10) REFERENCES Courses(course_id),
>     total_marks_obtained NUMERIC(5,2) NOT NULL,
>     UNIQUE (student_id, course_id) -- A student has one result per course
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

## Summer Semester 2018-2019

> [!question] Question 1(c): Foreign Key & Primary Key
> Mention two important purposes of a foreign key with suitable examples. Explain the guidelines to select a primary key.

> [!info]- Answer
> **Foreign Key Purposes:**
> 1. **Referential Integrity:** Ensures that a value in one table corresponds to an existing valid value in another. (e.g., An `emp_id` in `Results` must exist in `Students`).
> 2. **Relationship Enforcement:** Links tables together (e.g., `department_id` in `Employee` links an employee to a specific `Department`).
> 
> **Primary Key Guidelines:**
> - Must be unique and NOT NULL.
> - Should rarely or never change.
> - As compact as possible (e.g., an Integer ID rather than a long string).
> - Should ideally be meaningless (surrogate key) to avoid business logic changes affecting the key.

> [!question] Question 2(a): SQL Queries (DEPT and EMP)
> i: Write SQL to find the list of employee IDs, names, manager's names, and salary status (High: >100,000; Moderate: 50,000–100,000; Low: <50,000).
> ii: Write SQL to find the list of department names and establish dates along with the total number of employees in each.
> iii: Write SQL to list all employees with their first name and total salary (calculated as basic + 40% house rent + 10% transport + 150% commission bonus) in decreasing order.

> [!info]- Answer
> ```sql
> -- i. Employee details and salary status
> SELECT e.emp_id, e.emp_name, m.emp_name AS manager_name,
>        CASE
>            WHEN e.salary > 100000 THEN 'High'
>            WHEN e.salary >= 50000 THEN 'Moderate'
>            ELSE 'Low'
>        END AS salary_status
> FROM EMP e
> LEFT JOIN EMP m ON e.manager_id = m.emp_id;
> 
> -- ii. Department details and employee count
> SELECT d.dept_name, d.establish_date, COUNT(e.emp_id) AS total_employees
> FROM DEPT d
> LEFT JOIN EMP e ON d.dept_id = e.dept_id
> GROUP BY d.dept_name, d.establish_date;
> 
> -- iii. First name and total salary
> SELECT split_part(emp_name, ' ', 1) AS first_name,
>        (basic + (basic * 0.40) + (basic * 0.10) + (basic * 1.50)) AS total_salary
> FROM EMP
> ORDER BY total_salary DESC;
> ```

> [!question] Question 2(b): Function getstatus
> Write a function `getstatus` that takes an ID and outputs a status (POOR, ORDINARY, or GOOD) based on total yearly salary thresholds.

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION getstatus(p_emp_id INT)
> RETURNS VARCHAR AS $$
> DECLARE
>     v_yearly_salary NUMERIC;
>     v_status VARCHAR(10);
> BEGIN
>     -- Calculate yearly salary (assumes basic is monthly)
>     SELECT (basic * 12) INTO v_yearly_salary
>     FROM EMP WHERE emp_id = p_emp_id;
>     
>     IF v_yearly_salary > 1200000 THEN
>         v_status := 'GOOD';
>     ELSIF v_yearly_salary >= 500000 THEN
>         v_status := 'ORDINARY';
>     ELSE
>         v_status := 'POOR';
>     END IF;
>     
>     RETURN v_status;
> END;
> $$ LANGUAGE plpgsql;
> ```

> [!question] Question 3(a): Banking Scenario DDLs
> Design the ER diagram and write the required DDLs for the banking scenario described. (Providing DDLs)

> [!info]- Answer
> ```sql
> CREATE TABLE Customers (
>     customer_id SERIAL PRIMARY KEY,
>     name VARCHAR(100) NOT NULL,
>     address TEXT
> );
> 
> CREATE TABLE Accounts (
>     account_no VARCHAR(20) PRIMARY KEY,
>     customer_id INT REFERENCES Customers(customer_id),
>     balance NUMERIC(15,2) DEFAULT 0
> );
> 
> CREATE TABLE Loans (
>     loan_id SERIAL PRIMARY KEY,
>     account_no VARCHAR(20) REFERENCES Accounts(account_no),
>     loan_scheme VARCHAR(50),
>     total_payback_amount NUMERIC(15,2)
> );
> ```

> [!question] Question 3(b): Loan Category Function
> Write a function to assign a customer to a specific category of loans (Platinum, Gold, Silver) based on eligibility parameters (total transactions in the last 12 months).

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION get_loan_category(p_customer_id INT)
> RETURNS VARCHAR AS $$
> DECLARE
>     v_total_tx_volume NUMERIC;
>     v_category VARCHAR(20);
> BEGIN
>     SELECT COALESCE(SUM(amount), 0) INTO v_total_tx_volume
>     FROM Transactions t
>     JOIN Accounts a ON t.account_no = a.account_no
>     WHERE a.customer_id = p_customer_id
>       AND t.tx_date >= CURRENT_DATE - INTERVAL '1 year';
>       
>     IF v_total_tx_volume >= 1000000 THEN
>         v_category := 'Platinum';
>     ELSIF v_total_tx_volume >= 500000 THEN
>         v_category := 'Gold';
>     ELSE
>         v_category := 'Silver';
>     END IF;
>     
>     RETURN v_category;
> END;
> $$ LANGUAGE plpgsql;
> ```

> [!question] Question 4(a): Procedure distribute_allowance
> Write a procedure `distribute_allowance` to distribute a government aid fund to citizens based on salary priority and application status until the fund is exhausted.

> [!info]- Answer
> ```sql
> CREATE OR REPLACE PROCEDURE distribute_allowance(p_total_fund NUMERIC)
> LANGUAGE plpgsql
> AS $$
> DECLARE
>     v_remaining_fund NUMERIC := p_total_fund;
>     v_citizen RECORD;
>     v_allowance_amount NUMERIC := 5000; -- Assuming a fixed amount
> BEGIN
>     FOR v_citizen IN (
>         SELECT citizen_id, salary 
>         FROM Citizens 
>         WHERE application_status = 'Approved'
>         ORDER BY salary ASC
>     ) LOOP
>         IF v_remaining_fund >= v_allowance_amount THEN
>             -- Distribute
>             INSERT INTO Disbursements (citizen_id, amount, disburse_date)
>             VALUES (v_citizen.citizen_id, v_allowance_amount, CURRENT_DATE);
>             
>             v_remaining_fund := v_remaining_fund - v_allowance_amount;
>         ELSE
>             EXIT; -- Fund exhausted
>         END IF;
>     END LOOP;
> END;
> $$;
> ```

## Summer Semester 2021-2022

> [!question] Question 1(a): Errors in GET_STATUS
> Identify and briefly explain the errors in the provided PL/SQL code snippet for the `GET_STATUS` function.

> [!info]- Answer
> *(Note: Same as 2023-2024 Question 2c)*
> Since we are following **PostgreSQL**, the snippet contains several Oracle-specific syntax errors and logic errors:
> 1. **Oracle specific types and keywords:** `varchar2` should be `varchar`. `return number(6,2)` should be `RETURNS numeric(6,2)`. `IS` should be `AS $$`.
> 2. **Size constraints on parameters:** `varchar(20)` should just be `varchar`.
> 3. **Modifying an IN parameter:** The parameter `p_name` is read-only.
> 4. **Missing RETURN statement:** The body lacks a `RETURN <value>;` statement.
> 5. **Missing Language Declaration:** Requires `$$ LANGUAGE plpgsql;` at the end.

> [!question] Question 1(b): `%TYPE` Advantage
> State the main advantage of using `%TYPE` over basic declarations inside a PL/SQL sub-program.

> [!info]- Answer
> The main advantage of `%TYPE` is **dynamic datatype adaptation**. If the underlying table's column datatype changes (e.g., from `VARCHAR(50)` to `VARCHAR(100)`), the PL/SQL variable declared with `%TYPE` will automatically inherit the new datatype without requiring any changes to the code. This prevents runtime errors and reduces maintenance overhead.

> [!question] Question 1(c): UPDATE Statement & Anonymous Block
> Write an `UPDATE` statement to decrease salaries by 25% for employees earning above the average. Then, write an anonymous block to execute this and print the count of updated records or a "NO RECORDS" message.

> [!info]- Answer
> ```sql
> DO $$
> DECLARE
>     v_count INT;
> BEGIN
>     UPDATE EMP
>     SET salary = salary * 0.75
>     WHERE salary > (SELECT AVG(salary) FROM EMP);
>     
>     -- Check how many rows were updated
>     GET DIAGNOSTICS v_count = ROW_COUNT;
>     
>     IF v_count > 0 THEN
>         RAISE NOTICE '% RECORDS UPDATED', v_count;
>     ELSE
>         RAISE NOTICE 'NO RECORDS UPDATED';
>     END IF;
> END;
> $$;
> ```

> [!question] Question 2(a): DDLs for Telecom Company
> Create the required DDL statements (tables for customers, SIMs, plans, and call logs) to design the system.

> [!info]- Answer
> ```sql
> CREATE TABLE Customers (
>     customer_id SERIAL PRIMARY KEY,
>     name VARCHAR(100) NOT NULL
> );
> 
> CREATE TABLE Plans (
>     plan_id SERIAL PRIMARY KEY,
>     plan_name VARCHAR(50),
>     pulse_rate NUMERIC(5,2) -- Cost per minute
> );
> 
> CREATE TABLE SIMs (
>     sim_no VARCHAR(15) PRIMARY KEY,
>     customer_id INT REFERENCES Customers(customer_id),
>     plan_id INT REFERENCES Plans(plan_id)
> );
> 
> CREATE TABLE CallLogs (
>     call_id VARCHAR(50) PRIMARY KEY,
>     sim_no VARCHAR(15) REFERENCES SIMs(sim_no),
>     duration_seconds INT,
>     call_time TIMESTAMP DEFAULT NOW()
> );
> ```

> [!question] Question 2(b): Calculate Call Charges
> Write a PL/SQL function to calculate call charges based on plan and duration (using a 1-minute pulse).

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION calculate_call_charge(p_sim_no VARCHAR, p_duration_sec INT)
> RETURNS NUMERIC AS $$
> DECLARE
>     v_pulse_rate NUMERIC;
>     v_minutes INT;
>     v_charge NUMERIC;
> BEGIN
>     -- Get plan rate
>     SELECT p.pulse_rate INTO v_pulse_rate
>     FROM SIMs s JOIN Plans p ON s.plan_id = p.plan_id
>     WHERE s.sim_no = p_sim_no;
>     
>     -- Calculate minutes (ceiling)
>     v_minutes := CEIL(p_duration_sec / 60.0);
>     
>     -- Calculate charge
>     v_charge := v_minutes * v_pulse_rate;
>     RETURN v_charge;
> END;
> $$ LANGUAGE plpgsql;
> ```

> [!question] Question 2(c): CallID Generation in Trigger
> Write a function to generate a `CallID` in the format `YYYYMMDD.NNNNNNNN` and place it in a suitable trigger.

> [!info]- Answer
> ```sql
> CREATE SEQUENCE call_id_seq START 1;
> 
> CREATE OR REPLACE FUNCTION generate_call_id()
> RETURNS TRIGGER AS $$
> DECLARE
>     v_date_part VARCHAR;
>     v_seq_part VARCHAR;
> BEGIN
>     v_date_part := TO_CHAR(CURRENT_DATE, 'YYYYMMDD');
>     -- Pad sequence with zeros to length 8
>     v_seq_part := LPAD(NEXTVAL('call_id_seq')::TEXT, 8, '0');
>     
>     NEW.call_id := v_date_part || '.' || v_seq_part;
>     RETURN NEW;
> END;
> $$ LANGUAGE plpgsql;
> 
> CREATE TRIGGER trigger_generate_call_id
> BEFORE INSERT ON CallLogs
> FOR EACH ROW
> EXECUTE FUNCTION generate_call_id();
> ```

> [!question] Question 3: Scholarship Disbursal Function
> Create a PL/SQL function that takes a total scholarship fund and per-student amount as input. It should disburse the fund based on CGPA and misconduct criteria and output the number of students who received it vs. those who were eligible but missed out.

> [!info]- Answer
> ```sql
> CREATE OR REPLACE FUNCTION disburse_scholarship(p_total_fund NUMERIC, p_amount NUMERIC)
> RETURNS TABLE (received_count INT, missed_count INT) AS $$
> DECLARE
>     v_remaining_fund NUMERIC := p_total_fund;
>     v_student RECORD;
>     v_received INT := 0;
>     v_missed INT := 0;
> BEGIN
>     FOR v_student IN (
>         SELECT student_id 
>         FROM Students 
>         WHERE has_misconduct = FALSE AND cgpa >= 3.50
>         ORDER BY cgpa DESC
>     ) LOOP
>         IF v_remaining_fund >= p_amount THEN
>             -- Logic to disburse (e.g., INSERT into awards table)
>             v_remaining_fund := v_remaining_fund - p_amount;
>             v_received := v_received + 1;
>         ELSE
>             v_missed := v_missed + 1;
>         END IF;
>     END LOOP;
>     
>     RETURN QUERY SELECT v_received, v_missed;
> END;
> $$ LANGUAGE plpgsql;
> ```

## Summer Semester 2022-2023

> [!question] Question 1(a): Many-to-Many Cardinality
> How do you implement many-to-many cardinality? Explain with an example.

> [!info]- Answer
> Many-to-many cardinality is implemented by introducing a **junction table** (or associative entity) that maps the keys of both participating tables.
> 
> *Example:* Students taking Courses. A student can take many courses, and a course can have many students.
> - Table `Students(student_id, name)`
> - Table `Courses(course_id, title)`
> - Junction Table `Enrollments(student_id, course_id)` where the primary key is the composite of `(student_id, course_id)`.

> [!question] Question 1(c): Procedure to Function Conversion
> "It is possible to convert each PL/SQL procedure into an equivalent function." Justify your position on this statement.

> [!info]- Answer
> Yes, it is functionally possible, because a function in PostgreSQL can do everything a procedure can do, and can optionally return `void`. However, there is one major exception in modern PostgreSQL: **Transaction Control**. 
> Procedures can use `COMMIT` and `ROLLBACK` within their execution block to manage partial transactions. Functions run entirely within a single transaction and cannot commit or rollback independently. So if the procedure relies on transaction control, an exact functional equivalent is not possible.

> [!question] Question 2(a): DDLs for PA & Internet Policy
> Create the required tables (student info, transactions, disciplinary records) maintaining standard design principles.

> [!info]- Answer
> ```sql
> CREATE TABLE StudentInfo (
>     student_id INT PRIMARY KEY,
>     name VARCHAR(100),
>     cgpa NUMERIC(3,2)
> );
> 
> CREATE TABLE DisciplinaryRecords (
>     record_id SERIAL PRIMARY KEY,
>     student_id INT REFERENCES StudentInfo(student_id),
>     event_date DATE,
>     description TEXT
> );
> 
> CREATE TABLE InternetUsage (
>     usage_id SERIAL PRIMARY KEY,
>     student_id INT REFERENCES StudentInfo(student_id),
>     usage_date DATE,
>     mb_used NUMERIC,
>     scheme VARCHAR(10) CHECK (scheme IN ('Day', 'Night'))
> );
> ```

> [!question] Question 3(b): `%TYPE` and Cursors
> What is the basic benefit of using `%TYPE`? Differentiate between implicit and explicit cursors.

> [!info]- Answer
> **Benefit of `%TYPE`:** Automatically adapts to schema changes in the base table, preventing hardcoded type mismatch errors.
> 
> **Implicit vs Explicit Cursors:**
> - **Implicit:** Managed automatically by PostgreSQL (e.g., in a `FOR row IN (SELECT...) LOOP`). Easy to use but gives less control.
> - **Explicit:** Manually defined and controlled using `DECLARE`, `OPEN`, `FETCH`, and `CLOSE`. Used for complex row-by-row processing, taking parameters, or using scrollable logic (`FETCH PRIOR`/`NEXT`).
