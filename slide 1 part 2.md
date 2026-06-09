# PL/pgSQL Complete Exam Preparation Guide

> Full notes with code + explanations + tricky exam strategies — IUT Database Systems

---

## Table of Contents

1. [What is PL/pgSQL?](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#1-what-is-plpgsql)
2. [Block Structure — Anonymous & Named](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#2-block-structure)
3. [Control Flow — IF, FOR, WHILE](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#3-control-flow)
4. [Arrays](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#4-arrays)
5. [Date & Time Arithmetic](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#5-date--time-arithmetic)
6. [Network Address Types](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#6-network-address-types-inet--cidr)
7. [Range Types & Canonical Form](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#7-range-types--canonical-form)
8. [GiST Indexes & Exclusion Constraints](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#8-gist-indexes--exclusion-constraints)
9. [Composite Types](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#9-composite-types)
10. [Domain Types](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#10-domain-types)
11. [Exam Traps Master Cheatsheet](https://claude.ai/chat/831db0e1-9473-4cff-bca6-2c8eeec6f3fe#11-exam-traps-master-cheatsheet)

---

## 1. What is PL/pgSQL?

Standard SQL is a **query language** — you ask for data, it gives it back. That's all it can do. It has no ability to loop, make decisions, or store temporary values.

**PL/pgSQL** (Procedural Language/PostgreSQL) is PostgreSQL's built-in programming language. It gives you `IF/ELSE`, `FOR` loops, `WHILE` loops, variables, functions — everything a normal programming language has — but running _inside_ the database itself.

**Why does this matter architecturally?**

Imagine your backend fetches 10,000 rows, loops through them in Python, does some calculation, and returns the result. That's:

- 10,000 rows traveling over the network
- Memory used on your app server
- Round-trip latency

With PL/pgSQL, you write the loop inside the database. The database does the heavy lifting and only returns **the final result**. Zero extra round-trips. This is why companies with high-load systems push business logic into the database layer.

**Check PL/pgSQL is installed:**

```sql
\dx
-- Look for "plpgsql" in the list
```

---

## 2. Block Structure

Every piece of PL/pgSQL code lives inside a **block**. There are two kinds.

---

### A. Anonymous Block (`DO`) — testing only, never saved

An anonymous block is a one-time-use script. It runs immediately and is discarded — it is never stored in the database. Use it when you want to test some logic quickly without creating a permanent function.

```sql
DO $$
DECLARE
    -- Declare all your variables here with their types
    x NUMERIC := 102.32;
    y NUMERIC := 23.43;
BEGIN
    -- Your logic goes here
    RAISE NOTICE 'The sum is %', x + y;
    -- % is the placeholder. The value of (x + y) replaces it.
END;
$$;
```

**How to read this structure:**

- `DO $$` — starts the anonymous block. The `$$` is a dollar-quote delimiter (avoids conflicts with single quotes inside the code).
- `DECLARE` — this section is for declaring variables. You must declare every variable here before using it in `BEGIN`.
- `BEGIN ... END;` — the actual logic runs here.
- `RAISE NOTICE` — prints a message to the output. This is your "print statement" in PL/pgSQL.
- `$$;` — closes the block.

> **Exam Tip:** If a question asks "which block type is NOT saved to the database?" — the answer is the anonymous `DO` block.

---

### B. Named Function — saved permanently, callable anytime

A named function is stored in the database permanently. You can call it from SQL, from your application, or from another function. This is the production-grade option.

```sql
CREATE OR REPLACE FUNCTION greet_student(student_name TEXT)
RETURNS TEXT AS $$
BEGIN
    RETURN 'Hello, ' || student_name || '! Good luck on your exam.';
END;
$$ LANGUAGE plpgsql;

-- Call it:
SELECT greet_student('Sami');
-- Returns: "Hello, Sami! Good luck on your exam."
```

**Anatomy breakdown:**

- `CREATE OR REPLACE FUNCTION` — creates the function. If it already exists, replaces it instead of throwing an error. Always use this form.
- `greet_student(student_name TEXT)` — the function name and its input parameters with types.
- `RETURNS TEXT` — declares what type this function gives back.
- `AS $$` — opens the function body with dollar-quoting.
- `RETURN` — sends the value back to the caller.
- `$$ LANGUAGE plpgsql` — closes the body and tells PostgreSQL which language this is.
- `||` — the string concatenation operator in SQL (like `+` in Python for strings).

> **Exam Tip:** `CREATE FUNCTION` fails if the function already exists. `CREATE OR REPLACE FUNCTION` is safe to run multiple times. In exams, prefer `CREATE OR REPLACE`.

---

## 3. Control Flow

### IF / ELSIF / ELSE

```sql
CREATE OR REPLACE FUNCTION grade_result(marks INTEGER)
RETURNS TEXT AS $$
BEGIN
    IF marks >= 80 THEN
        RETURN 'A+';
    ELSIF marks >= 70 THEN
        RETURN 'A';
    ELSIF marks >= 60 THEN
        RETURN 'B';
    ELSIF marks >= 50 THEN
        RETURN 'C';
    ELSE
        RETURN 'F';
    END IF;
END;
$$ LANGUAGE plpgsql;

SELECT grade_result(75);  -- Returns 'A'
SELECT grade_result(45);  -- Returns 'F'
```

**Key syntax rules:**

- `ELSIF` — not `ELSE IF` (no space). This is different from many other languages.
- `END IF;` — mandatory closing. Every `IF` must have a matching `END IF;`.
- Conditions are evaluated top-to-bottom. The first one that is `TRUE` wins — the rest are skipped.

> **Exam Trap:** Writing `ELSE IF` instead of `ELSIF` is a syntax error in PL/pgSQL. The examiner may present both as options.

---

### FOR Loop

A `FOR` loop iterates over a range of integers. Syntax: `FOR variable IN start..end LOOP`.

```sql
CREATE OR REPLACE FUNCTION sum_upto(n INTEGER)
RETURNS INTEGER AS $$
DECLARE
    total INTEGER := 0;
BEGIN
    -- i goes from 1 to n, incrementing by 1 each time
    FOR i IN 1..n LOOP
        total := total + i;
    END LOOP;

    RETURN total;
END;
$$ LANGUAGE plpgsql;

SELECT sum_upto(10);  -- Returns 55  (1+2+3+...+10)
SELECT sum_upto(5);   -- Returns 15  (1+2+3+4+5)
```

**Note:** You do NOT declare the loop variable `i` in the `DECLARE` section. PL/pgSQL declares it automatically as an `INTEGER` for the duration of the loop.

**Reverse loop:** Add `REVERSE` to iterate backwards.

```sql
DO $$
BEGIN
    FOR i IN REVERSE 5..1 LOOP
        RAISE NOTICE 'Countdown: %', i;
    END LOOP;
END;
$$;
-- Output: 5, 4, 3, 2, 1
```

---

### WHILE Loop

A `WHILE` loop keeps running as long as its condition is `TRUE`.

```sql
CREATE OR REPLACE FUNCTION count_down(start_val INTEGER)
RETURNS VOID AS $$
DECLARE
    counter INTEGER := start_val;
BEGIN
    WHILE counter > 0 LOOP
        RAISE NOTICE 'Count: %', counter;
        counter := counter - 1;   -- must manually update counter or loop runs forever!
    END LOOP;
END;
$$ LANGUAGE plpgsql;

SELECT count_down(3);
-- Output: Count: 3 → Count: 2 → Count: 1
```

> **`RETURNS VOID`** — means the function does work (printing, inserting data) but returns nothing to the caller. You still call it with `SELECT count_down(3)`.

> **Exam Trap:** If you forget to update `counter` inside a WHILE loop (`counter := counter - 1`), you get an **infinite loop**. Examiners may ask you to identify the bug in broken code.

---

## 4. Arrays

### Declaration and Access

```sql
DO $$
DECLARE
    -- Define an array with INTEGER[] or TEXT[] etc.
    scores  INTEGER[] := ARRAY[90, 85, 78, 92, 65];
    courses TEXT[]   := ARRAY['Database', 'OS', 'Networks', 'AI'];

    first_score  INTEGER;
    total_items  INTEGER;
BEGIN
    -- Access elements — PostgreSQL arrays start at index 1, NOT 0
    first_score := scores[1];                  -- 90  (first element)
    total_items := array_length(scores, 1);    -- 5   (the '1' means first dimension)

    RAISE NOTICE 'First score: %', first_score;
    RAISE NOTICE 'Total items: %', total_items;

    -- What happens at index 0?
    RAISE NOTICE 'Index 0 gives: %', scores[0];  -- NULL (no error, just NULL!)
END;
$$;
```

**Why `array_length(arr, 1)`?** The second argument is the _dimension_. PostgreSQL supports multi-dimensional arrays (like a 2D matrix). For a normal 1D array, always pass `1`.

### Looping Through an Array

```sql
DO $$
DECLARE
    courses TEXT[] := ARRAY['Database', 'OS', 'Networks', 'AI'];
    course  TEXT;
BEGIN
    -- FOREACH is the clean way to loop over arrays
    FOREACH course IN ARRAY courses LOOP
        RAISE NOTICE 'Course: %', course;
    END LOOP;
END;
$$;
-- Output: Database → OS → Networks → AI
```

### Adding Elements to an Array

```sql
DO $$
DECLARE
    nums INTEGER[] := ARRAY[1, 2, 3];
BEGIN
    -- Use array_append() to add to the end
    nums := array_append(nums, 4);
    RAISE NOTICE 'Array: %', nums;  -- {1,2,3,4}
END;
$$;
```

> **⚠️ Critical Exam Trap — 1-Indexed Arrays**
> 
> This is the #1 most-tested array fact. PostgreSQL arrays are **1-indexed**.
> 
> - `arr[1]` → first element ✓
> - `arr[0]` → returns `NULL` silently (no error thrown) ✗
> 
> If an exam question shows `arr[0]` and asks "what is the output?", the answer is **NULL**, not the first element and not an error.

---

## 5. Date & Time Arithmetic

### INTERVAL — Adding and Subtracting Time

`INTERVAL` lets you do direct math on dates without any complicated functions.

```sql
DO $$
DECLARE
    enroll_date  DATE := '2024-01-15';
    grad_date    DATE;
    exam_date    DATE;
    deadline     TIMESTAMPTZ;
BEGIN
    -- Add intervals directly to dates
    grad_date := enroll_date + INTERVAL '4 years';
    exam_date := CURRENT_DATE + INTERVAL '7 days';
    deadline  := NOW() + INTERVAL '2 hours 30 minutes';

    RAISE NOTICE 'Graduation: %', grad_date;
    RAISE NOTICE 'Exam date: %', exam_date;
    RAISE NOTICE 'Deadline: %', deadline;
END;
$$;
```

**INTERVAL formats:**

- `'5 days'`
- `'3 months'`
- `'1 year'`
- `'2 hours 30 minutes'`
- `'1 year 3 months 5 days'` — you can combine them

### EXTRACT — Pulling Out One Part of a Date

```sql
DO $$
DECLARE
    current_year  INTEGER;
    current_month INTEGER;
    day_of_week   INTEGER;
BEGIN
    current_year  := EXTRACT(YEAR  FROM CURRENT_DATE);
    current_month := EXTRACT(MONTH FROM CURRENT_DATE);
    day_of_week   := EXTRACT(DOW   FROM CURRENT_DATE);  -- 0=Sunday, 6=Saturday

    RAISE NOTICE 'Year: %, Month: %, Day of week: %',
                  current_year, current_month, day_of_week;
END;
$$;
```

### AGE — Human-Readable Difference Between Dates

```sql
-- AGE(end_date, start_date) — order matters! Later date first.
SELECT AGE('2026-06-10', '2024-01-15');
-- Returns: "2 years 4 mons 26 days"

-- Calculate someone's age
SELECT AGE(CURRENT_DATE, '2001-05-20');
-- Returns: how long ago that birthday was
```

> **Exam Tip:** `AGE(later, earlier)` — the more recent date goes **first**. Reversed gives a negative interval.

**Date arithmetic quick reference:**

|Operation|Example|What it returns|
|---|---|---|
|Add days|`CURRENT_DATE + INTERVAL '5 days'`|Date 5 days later|
|Subtract dates|`'2026-01-01'::DATE - CURRENT_DATE`|Number of days between|
|Get year|`EXTRACT(YEAR FROM CURRENT_DATE)`|Integer year|
|Human diff|`AGE('2026-01-01', '2023-10-21')`|`"2 years 2 mons 11 days"`|

---

## 6. Network Address Types (INET & CIDR)

### The Problem With Storing IPs as TEXT

If you store IP addresses as `TEXT`, three things break:

1. **No validation** — garbage like `'10.20.30.1o1'` (letter 'o' not zero) gets stored silently
2. **Wrong sorting** — text sorts character-by-character, so `"10.0.0.10"` comes before `"10.0.0.2"` alphabetically, which is wrong
3. **No subnet logic** — you can't ask "is this IP inside this network?" with text operators

### INET vs CIDR

|Type|Stores|Example|
|---|---|---|
|`INET`|A specific host IP address|`192.168.1.50`|
|`CIDR`|A network block / subnet|`192.168.1.0/24` (256 addresses)|

```sql
CREATE TABLE server_log (
    log_id    SERIAL PRIMARY KEY,
    client_ip INET,         -- where the request came from
    network   CIDR,         -- which subnet it belongs to
    logged_at TIMESTAMPTZ DEFAULT NOW()
);

-- Valid inserts
INSERT INTO server_log (client_ip, network)
VALUES ('192.168.1.50', '192.168.1.0/24');

INSERT INTO server_log (client_ip, network)
VALUES ('10.0.0.1', '10.0.0.0/8');

-- This gets REJECTED — 'o' is not a valid digit
INSERT INTO server_log (client_ip) VALUES ('10.20.30.1o1'); -- ERROR!

-- Subnet containment: find all IPs inside a specific network
SELECT client_ip FROM server_log
WHERE client_ip << '192.168.1.0/24';
-- << means "is contained within"
-- Returns: 192.168.1.50 (because it's in that /24 block)

-- Sorting works correctly (by numerical value, not alphabetical)
SELECT client_ip FROM server_log ORDER BY client_ip;
```

### Network Operators

|Operator|Meaning|Example|
|---|---|---|
|`<<`|IP is contained within subnet|`'192.168.1.50' << '192.168.1.0/24'` → TRUE|
|`>>`|Subnet contains IP|`'192.168.1.0/24' >> '192.168.1.50'` → TRUE|
|`=`|Exact match|`client_ip = '10.0.0.1'`|

> **Exam Tip:** Questions may ask "which data type should you use to store an IP address?" The answer is `INET` (for a specific address) or `CIDR` (for a subnet), never `TEXT`. The three reasons: validation, sorting, subnet operators.

---

## 7. Range Types & Canonical Form

A **range type** stores a span of values in a single column. This is perfect for time slots, price ranges, version numbers, and anything involving "from X to Y".

### Built-in Range Types

|Type|For|Example|
|---|---|---|
|`int4range`|Integer ranges|`[1, 10)`|
|`numrange`|Decimal/numeric ranges|`[1.5, 9.9]`|
|`tsrange`|Timestamp ranges (no timezone)|`[09:00, 11:00)`|
|`tstzrange`|Timestamp ranges (with timezone)|`[09:00+06, 11:00+06)`|
|`daterange`|Date ranges|`[2024-01-01, 2024-12-31]`|

### Bracket Notation

```
[  = inclusive (the boundary value IS included)
(  = exclusive (the boundary value is NOT included)
]  = inclusive (right side)
)  = exclusive (right side)
```

```sql
-- Integer range examples
SELECT int4range(1, 10, '[]');  -- includes both 1 and 10 → stored as [1, 11)
SELECT int4range(1, 10, '[)');  -- includes 1, excludes 10 → [1, 10)  (1 to 9)
SELECT int4range(1, 10, '(]');  -- excludes 1, includes 10 → [2, 11)  (2 to 10)
SELECT int4range(1, 10, '()');  -- excludes both → [2, 10)  (2 to 9)
```

### The Canonical Form Rule — ⚠️ Most Tricky Exam Concept

When you insert `int4range(1, 10, '[]')` — which logically means "integers 1 through 10 inclusive" — PostgreSQL does NOT store it as `[1, 10]`.

**It rewrites it to `[1, 11)`.**

**Why?** Integers are _discrete_ — you can list every one of them: 1, 2, 3, 4, 5, 6, 7, 8, 9, 10. There is no integer between 10 and 11. So "all integers from 1 to 10 inclusive" is **mathematically identical** to "all integers starting at 1, up to but not including 11." PostgreSQL standardises to the `[)` form (inclusive start, exclusive end) to make index operations faster and consistent.

```sql
-- Demonstrate canonical form
SELECT int4range(1, 10, '[]');   -- Output: [1,11)   ← rewritten!
SELECT int4range(1, 10, '[)');   -- Output: [1,10)   ← already canonical, unchanged
SELECT int4range(2, 11, '()');   -- Output: [3,11)   ← rewritten! (excludes 2, excludes 11)

-- numrange does NOT get rewritten — decimals are continuous
SELECT numrange(1.0, 10.0, '[]');  -- Output: [1.0,10.0]  ← stays as-is
```

> **Exam Trap — Canonical Form Questions**
> 
> If the exam asks: "You insert `int4range(1, 10, '[]')`. What is stored?" Answer: `[1, 11)` — NOT `[1, 10]`.
> 
> If the exam asks: "You insert `numrange(1.0, 10.0, '[]')`. What is stored?" Answer: `[1.0, 10.0]` — unchanged. Continuous types are NOT rewritten.
> 
> The rule only applies to **discrete** types (integer ranges).

### Range Operators

```sql
-- && : Do these ranges OVERLAP? (share any values)
SELECT int4range(1, 10) && int4range(8, 15);  -- TRUE  (share 8 and 9)
SELECT int4range(1, 5)  && int4range(6, 10);  -- FALSE (no shared values)

-- @> : Does the left range CONTAIN the right value or range?
SELECT int4range(1, 10) @> 5;               -- TRUE  (5 is inside [1,10))
SELECT int4range(1, 10) @> int4range(3, 7); -- TRUE  ([3,7) fits inside [1,10))
SELECT int4range(1, 10) @> 10;              -- FALSE (10 is NOT in [1,10))

-- -|- : Are they ADJACENT? (touching but not overlapping)
SELECT int4range(1, 5)  -|- int4range(5, 10);  -- TRUE  ([1,5) ends at 5, [5,10) starts at 5)
SELECT int4range(1, 5)  -|- int4range(6, 10);  -- FALSE (gap between 5 and 6)
SELECT int4range(1, 10) -|- int4range(8, 15);  -- FALSE (they overlap, not adjacent)
```

> **Exam Trick — Adjacency vs Overlap:** `[1, 5)` and `[5, 10)` are **adjacent** — NOT overlapping. The first range ends right before 5 (exclusive), and the second starts at 5 (inclusive). They touch perfectly with no shared value. But `[1, 5]` and `[5, 10]` **DO overlap** — both include the value 5.

---

## 8. GiST Indexes & Exclusion Constraints

### Why B-Tree Indexes Fail for Ranges

The default index type in PostgreSQL is **B-Tree**. It is built for simple comparisons: equal, less than, greater than. It cannot answer the question "does this range overlap with any existing range?" — which is exactly what you need for a booking system.

**GiST (Generalized Search Tree)** is a special index framework designed for complex data types including geometric shapes, full-text search, and — importantly — ranges. It can efficiently check for overlap.

### Building a Double-Booking Prevention System

```sql
-- Step 1: Create the table with a timestamp range column
CREATE TABLE room_bookings (
    booking_id SERIAL PRIMARY KEY,
    room_name  TEXT      NOT NULL,
    booked_by  TEXT      NOT NULL,
    time_slot  TSRANGE   NOT NULL   -- stores start and end time together
);

-- Step 2: GiST index — required for the exclusion constraint to work
CREATE INDEX idx_booking_gist ON room_bookings USING GIST (time_slot);

-- Step 3: Exclusion constraint — this is where the magic happens
-- "Reject an INSERT if there's already a row where room_name is EQUAL
--  AND time_slot OVERLAPS with the new row's time_slot"
ALTER TABLE room_bookings
ADD CONSTRAINT no_double_booking
EXCLUDE USING GIST (
    room_name WITH =,    -- same room...
    time_slot WITH &&    -- ...AND overlapping time = CONFLICT
);

-- Step 4: Insert some test bookings
INSERT INTO room_bookings (room_name, booked_by, time_slot)
VALUES ('Lab-A', 'Sami', '[2026-06-10 09:00, 2026-06-10 11:00)');
-- ✓ Succeeds — no conflict

INSERT INTO room_bookings (room_name, booked_by, time_slot)
VALUES ('Lab-A', 'Rahim', '[2026-06-10 13:00, 2026-06-10 15:00)');
-- ✓ Succeeds — different time, no overlap

INSERT INTO room_bookings (room_name, booked_by, time_slot)
VALUES ('Lab-A', 'Karim', '[2026-06-10 10:00, 2026-06-10 12:00)');
-- ✗ ERROR: conflicting key value violates exclusion constraint "no_double_booking"
-- Because 10:00–12:00 overlaps with existing 09:00–11:00

INSERT INTO room_bookings (room_name, booked_by, time_slot)
VALUES ('Lab-B', 'Karim', '[2026-06-10 10:00, 2026-06-10 12:00)');
-- ✓ Succeeds — different room (Lab-B), so no conflict even with overlapping time
```

**Why this is better than application-level checks:** Without an exclusion constraint, you would need to: (1) query for conflicts, (2) check the result in code, (3) insert if clear. But between steps 2 and 3, another user could insert the same slot — a **race condition**. The exclusion constraint is atomic — the check and the insert happen in one operation at the database level.

> **Exam Tip:** Questions may ask "what index type do you need for an exclusion constraint on a range column?" Answer: **GiST**. B-Tree does not support overlap checks.

---

## 9. Composite Types

A **composite type** is exactly like a `struct` in C++. It groups multiple related fields into a single named type, which you can then use as a column type in any table.

### Why Use Composite Types?

Without them, you store an address like this:

```sql
CREATE TABLE students (
    id          SERIAL,
    house_no    TEXT,
    road        TEXT,
    district    TEXT,
    post_code   INTEGER
);
-- These 4 address fields are separate, with no grouping logic
```

With a composite type, you group them conceptually:

```sql
-- Step 1: Define the composite type (the struct)
CREATE TYPE address_t AS (
    house_no   TEXT,
    road       TEXT,
    district   TEXT,
    post_code  INTEGER
);

-- Step 2: Use it as a single column
CREATE TABLE students (
    id      SERIAL PRIMARY KEY,
    name    TEXT,
    address address_t    -- one column that internally holds 4 fields
);

-- Step 3: Insert data using the ROW() constructor
INSERT INTO students (name, address)
VALUES ('Sami', ROW('42', 'Mirpur Road', 'Dhaka', 1216));

-- Step 4: Query nested fields using dot notation with parentheses
SELECT
    name,
    (address).district,   -- parentheses are REQUIRED
    (address).post_code
FROM students;

-- Step 5: Update a specific nested field
UPDATE students
SET address.road = 'Uttara Main Road'
WHERE name = 'Sami';
```

### Using Composite Types Inside PL/pgSQL Functions

```sql
CREATE OR REPLACE FUNCTION make_address(
    h TEXT, r TEXT, d TEXT, p INTEGER
)
RETURNS address_t AS $$
DECLARE
    result address_t;
BEGIN
    result.house_no  := h;
    result.road      := r;
    result.district  := d;
    result.post_code := p;
    RETURN result;
END;
$$ LANGUAGE plpgsql;

-- Call it:
SELECT make_address('10', 'Banani Road', 'Dhaka', 1213);
```

> **⚠️ Exam Trap — Parentheses in Dot Notation**
> 
> When accessing composite fields in a `SELECT`, you **must** wrap the column name in parentheses:
> 
> ```sql
> SELECT (address).district FROM students;  -- ✓ CORRECT
> SELECT address.district   FROM students;  -- ✗ ERROR: treats 'address' as a table alias
> ```
> 
> Without the parentheses, PostgreSQL thinks you're referencing a column named `district` in a table aliased as `address`. This causes an error or returns unexpected results.

---

## 10. Domain Types

A **Domain** is a base data type (like `TEXT`, `INTEGER`, `NUMERIC`) that has a `CHECK` constraint permanently attached to it. Once defined, you use it exactly like a built-in type, but it auto-enforces the rule on every insert and update.

### The Problem Domains Solve

Suppose you have 5 tables that all need a "valid department" column. Without a domain:

```sql
-- You write this same constraint 5 times, on 5 tables
dept TEXT CHECK (dept IN ('CSE', 'EEE', 'MPE'))
```

If the rule changes (add 'BBA'), you must `ALTER TABLE` 5 times. Miss one and your data becomes inconsistent.

With a domain, you define it once and use it everywhere:

```sql
-- Create the domain — define the rule once
CREATE DOMAIN dept_name AS TEXT
    CONSTRAINT valid_dept
    CHECK (VALUE IN ('CSE', 'EEE', 'MPE'));
-- VALUE is a keyword meaning "whatever value is being inserted"

-- Create the domain for a BD phone number
CREATE DOMAIN bd_phone AS TEXT
    CONSTRAINT valid_bd_phone
    CHECK (VALUE ~ '^01[3-9][0-9]{8}$');
-- ~ is the regex match operator
-- Regex means: starts with '01', then a digit 3-9, then exactly 8 more digits

-- Create a domain for positive amounts
CREATE DOMAIN positive_amount AS NUMERIC
    CONSTRAINT must_be_positive
    CHECK (VALUE > 0);

-- Now use all three like native types
CREATE TABLE faculty (
    id     SERIAL PRIMARY KEY,
    name   TEXT NOT NULL,
    phone  bd_phone,           -- auto-validates phone format
    dept   dept_name,          -- auto-rejects anything not CSE/EEE/MPE
    salary positive_amount     -- auto-rejects zero or negative salaries
);

CREATE TABLE course_fees (
    course_id SERIAL,
    fee       positive_amount  -- same domain reused here — no duplicate constraint!
);

-- Test the constraints
INSERT INTO faculty (name, phone, dept, salary)
VALUES ('Dr. Rahman', '01712345678', 'CSE', 75000);
-- ✓ Succeeds

INSERT INTO faculty (name, phone, dept, salary)
VALUES ('Dr. Ahmed', '99999', 'CSE', 75000);
-- ✗ ERROR: phone fails the regex check

INSERT INTO faculty (name, phone, dept, salary)
VALUES ('Dr. Ali', '01712345678', 'BBA', 75000);
-- ✗ ERROR: 'BBA' is not in ('CSE', 'EEE', 'MPE')

INSERT INTO faculty (name, phone, dept, salary)
VALUES ('Dr. Khan', '01712345678', 'CSE', -5000);
-- ✗ ERROR: salary violates the positive_amount check
```

### Composite vs Domain — The Key Distinction

This comparison is the most commonly tested conceptual difference:

||Composite Type|Domain Type|
|---|---|---|
|**What it does**|**Groups** multiple fields together into one type|**Restricts** a single base type with a rule|
|**Analogy**|C++ `struct` or Python `dataclass`|An `int` that only accepts values 1–100|
|**Created with**|`CREATE TYPE name AS (field1 type, field2 type, ...)`|`CREATE DOMAIN name AS base_type CHECK (...)`|
|**Holds**|Multiple values of different types|One value of the base type|
|**Accessed with**|`(column).fieldname` dot notation|Used directly, like `TEXT` or `INTEGER`|
|**Use case**|Grouping: address, full name, coordinates|Enforcing: valid dept codes, phone formats|

> **Exam Trick:** If a question says "which type groups multiple fields?" → **Composite**. If it says "which type enforces a constraint on a single column that can be reused?" → **Domain**.

---

## 11. Exam Traps Master Cheatsheet

This section lists every tricky scenario your examiner is likely to use.

---

### Trap 1 — Array Index Starting at 1

```sql
DECLARE arr INTEGER[] := ARRAY[10, 20, 30];
BEGIN
    RAISE NOTICE '%', arr[0];  -- NULL  (no error!)
    RAISE NOTICE '%', arr[1];  -- 10    (correct first element)
    RAISE NOTICE '%', arr[3];  -- 30    (last element)
END;
```

**Question form:** "What does `arr[0]` return?" → Answer: `NULL`

---

### Trap 2 — Canonical Form for Integer Ranges

```sql
SELECT int4range(1, 10, '[]');  -- [1, 11)   ← NOT [1, 10]
SELECT int4range(1, 10, '(]');  -- [2, 11)   ← NOT (1, 10]
SELECT numrange(1.0, 10.0, '[]');  -- [1.0, 10.0]  ← stays unchanged
```

**Question form:** "What is stored when you insert `int4range(1,10,'[]')`?" → `[1,11)`

---

### Trap 3 — ELSIF Not ELSE IF

```sql
-- WRONG — syntax error
IF x > 10 THEN ...
ELSE IF x > 5 THEN ...   -- ✗ This is wrong!
END IF;

-- CORRECT
IF x > 10 THEN ...
ELSIF x > 5 THEN ...     -- ✓ One word: ELSIF
END IF;
```

---

### Trap 4 — Composite Field Access Needs Parentheses

```sql
-- WRONG — PostgreSQL thinks 'address' is a table alias
SELECT address.district FROM students;   -- ✗

-- CORRECT
SELECT (address).district FROM students; -- ✓
```

---

### Trap 5 — Adjacency vs Overlap in Range Operators

```sql
-- These look similar but give DIFFERENT results:
SELECT int4range(1,5) && int4range(5,10);    -- FALSE (no shared value: [1,5) ends before 5)
SELECT int4range(1,5) -|- int4range(5,10);   -- TRUE  (adjacent: they touch exactly at 5)

SELECT int4range(1,6) && int4range(5,10);    -- TRUE  (overlap: both include 5)
SELECT int4range(1,6) -|- int4range(5,10);   -- FALSE (not adjacent: they overlap)
```

**Rule:** If two ranges overlap (`&&` is TRUE), they cannot be adjacent (`-|-` is FALSE), and vice versa.

---

### Trap 6 — B-Tree vs GiST for Range Queries

**Question form:** "You need an exclusion constraint to prevent overlapping bookings. What index type do you use?"

- Wrong answer: B-Tree (the default)
- Correct answer: **GiST**

B-Tree handles `=`, `<`, `>`. GiST handles `&&` (overlap), `@>` (contains), `-|-` (adjacent).

---

### Trap 7 — INET Rejects Bad Data, TEXT Does Not

```sql
-- With TEXT column — bad data is accepted silently
INSERT INTO log (ip TEXT) VALUES ('10.20.30.1o1');  -- ✓ No error! Bad data stored.

-- With INET column — bad data is rejected
INSERT INTO log (ip INET) VALUES ('10.20.30.1o1');  -- ✗ ERROR: invalid input syntax for type inet
```

---

### Trap 8 — FOR Loop Variable Not Declared in DECLARE

```sql
-- WRONG — declaring the loop variable manually
DECLARE
    i INTEGER;   -- Don't do this for a FOR loop variable
BEGIN
    FOR i IN 1..5 LOOP ...

-- CORRECT — PL/pgSQL handles the FOR variable automatically
DECLARE
    total INTEGER := 0;   -- Only declare what YOU need
BEGIN
    FOR i IN 1..5 LOOP    -- i is auto-created as a local loop variable
        total := total + i;
    END LOOP;
```

---

### Trap 9 — Domain Uses `VALUE`, Composite Has Named Fields

```sql
-- Domain: use the keyword VALUE to refer to what's being inserted
CREATE DOMAIN positive_num AS INTEGER
    CHECK (VALUE > 0);    -- VALUE = the number being inserted

-- Composite: fields have actual names
CREATE TYPE coords AS (
    x NUMERIC,
    y NUMERIC
);
-- No CHECK constraint here — just field definitions
```

---

### Trap 10 — `RETURNS VOID` vs `RETURNS NULL`

`RETURNS VOID` means the function is designed to not return a value at all — like a procedure. It still must be called with `SELECT function_name()`. Returning `NULL` from any function is different — it means the function returned a value, and that value happened to be NULL.

---

### Quick Syntax Reference Card

| Concept          | Syntax                                                                                    |
| ---------------- | ----------------------------------------------------------------------------------------- |
| Anonymous block  | `DO $$ DECLARE ... BEGIN ... END; $$;`                                                    |
| Named function   | `CREATE OR REPLACE FUNCTION name(param type) RETURNS type AS $$ ... $$ LANGUAGE plpgsql;` |
| Assign variable  | `variable := value;`                                                                      |
| Print message    | `RAISE NOTICE 'text %', variable;`                                                        |
| If/else          | `IF ... THEN ... ELSIF ... THEN ... ELSE ... END IF;`                                     |
| For loop         | `FOR i IN 1..n LOOP ... END LOOP;`                                                        |
| Reverse loop     | `FOR i IN REVERSE n..1 LOOP ... END LOOP;`                                                |
| While loop       | `WHILE condition LOOP ... END LOOP;`                                                      |
| Array length     | `array_length(arr, 1)`                                                                    |
| Array loop       | `FOREACH item IN ARRAY arr LOOP ... END LOOP;`                                            |
| Composite insert | `ROW(val1, val2, val3)`                                                                   |
| Composite access | `(column_name).field_name`                                                                |
| Date add         | `date_column + INTERVAL '5 days'`                                                         |
| Extract part     | `EXTRACT(YEAR FROM date_column)`                                                          |
| Date diff        | `AGE(later_date, earlier_date)`                                                           |
| Subnet check     | `ip_column << '192.168.1.0/24'`                                                           |
| Range overlap    | `range1 && range2`                                                                        |
| Range contains   | `range1 @> value`                                                                         |
| Range adjacent   | `range1 -\|- range2`                                                                      |

---
