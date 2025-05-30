# Postgresql-Assignment-CMD-Output:
```sql
C:\Users\Minfy>psql
Password for user Minfy:

psql: error: connection to server at "localhost" (::1), port 5432 failed: FATAL:  password authentication failed for user "Minfy"

C:\Users\Minfy>psql -U postgres
Password for user postgres:

psql (17.5)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

postgres=# CREATE USER university_db WITH PASSWORD '01234';
CREATE ROLE
postgres=# CREATE DATABASE university OWNER university_db;
CREATE DATABASE
postgres=# CREATE USER university_db WITH PASSWORD '01234';
ERROR:  role "university_db" already exists
postgres=# CREATE DATABASE university OWNER university_db;
ERROR:  database "university" already exists
postgres=# /l
postgres-# ;
ERROR:  syntax error at or near "/"
LINE 1: /l
        ^
postgres=# \
invalid command \
Try \? for help.
postgres=# \l
                                                                       List of databases
    Name    |     Owner     | Encoding | Locale Provider |          Collate           |           Ctype            | Locale | ICU Rules |   Access privileges
------------+---------------+----------+-----------------+----------------------------+----------------------------+--------+-----------+-----------------------
 postgres   | postgres      | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           |
 template0  | postgres      | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           | =c/postgres          +
            |               |          |                 |                            |                            |        |           | postgres=CTc/postgres
 template1  | postgres      | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           | =c/postgres          +
            |               |          |                 |                            |                            |        |           | postgres=CTc/postgres
 university | university_db | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           |
(4 rows)


postgres=# GRANT ALL PRIVILEGES ON DATABASE university TO university_db;
GRANT
postgres=# \l
                                                                    List of databases
   Name    |  Owner   | Encoding | Locale Provider |          Collate           |           Ctype            | Locale | ICU Rules |   Access privileges
-----------+----------+----------+-----------------+----------------------------+----------------------------+--------+-----------+-----------------------
 postgres  | postgres | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           |
 template0 | postgres | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           | =c/postgres          +
           |          |          |                 |                            |                            |        |           | postgres=CTc/postgres
 template1 | postgres | UTF8     | libc            | English_United States.1252 | English_United States.1252 |        |           | =c/postgres          +
           |          |          |                 |                            |                            |        |           | postgres=CTc/postgres
(3 rows)


postgres=# CREATE USER university_admin WITH PASSWORD '01234';
CREATE ROLE
postgres=# CREATE DATABASE university_db OWNER university_admin;
CREATE DATABASE
postgres=# GRANT ALL PRIVILEGES ON DATABASE university_db TO university_admin;
GRANT
postgres=# psql -U university_admin -d university_db
postgres-# ;
ERROR:  syntax error at or near "psql"
LINE 1: psql -U university_admin -d university_db
        ^
postgres=# exit;

C:\Users\Minfy>psql -U university_admin -d university_db
Password for user university_admin:

psql (17.5)
WARNING: Console code page (437) differs from Windows code page (1252)
         8-bit characters might not work correctly. See psql reference
         page "Notes for Windows users" for details.
Type "help" for help.

university_db=> CREATE TABLE students (
university_db(>     student_id INTEGER,
university_db(>     first_name VARCHAR(50),
university_db(>     last_name VARCHAR(50),
university_db(>     email VARCHAR(100),
university_db(>     date_of_birth DATE
university_db(> );
CREATE TABLE
university_db=> ALTER TABLE students ADD COLUMN enrollment_date DATE;
ALTER TABLE
university_db=> \dt
              List of relations
 Schema |   Name   | Type  |      Owner
--------+----------+-------+------------------
 public | students | table | university_admin
(1 row)


university_db=> \d students
                          Table "public.students"
     Column      |          Type          | Collation | Nullable | Default
-----------------+------------------------+-----------+----------+---------
 student_id      | integer                |           |          |
 first_name      | character varying(50)  |           |          |
 last_name       | character varying(50)  |           |          |
 email           | character varying(100) |           |          |
 date_of_birth   | date                   |           |          |
 enrollment_date | date                   |           |          |


university_db=> ALTER TABLE students DROP COLUMN enrollment_date;
ALTER TABLE
university_db=> \d students
                         Table "public.students"
    Column     |          Type          | Collation | Nullable | Default
---------------+------------------------+-----------+----------+---------
 student_id    | integer                |           |          |
 first_name    | character varying(50)  |           |          |
 last_name     | character varying(50)  |           |          |
 email         | character varying(100) |           |          |
 date_of_birth | date                   |           |          |


university_db=> ALTER TABLE students ALTER COLUMN email TYPE VARCHAR(150);
ALTER TABLE
university_db=> \d students
                         Table "public.students"
    Column     |          Type          | Collation | Nullable | Default
---------------+------------------------+-----------+----------+---------
 student_id    | integer                |           |          |
 first_name    | character varying(50)  |           |          |
 last_name     | character varying(50)  |           |          |
 email         | character varying(150) |           |          |
 date_of_birth | date                   |           |          |


university_db=> ALTER TABLE students RENAME COLUMN date_of_birth TO dob;
ALTER TABLE
university_db=> \d students
                       Table "public.students"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(150) |           |          |
 dob        | date                   |           |          |


university_db=> ALTER TABLE students ADD CONSTRAINT unique_email UNIQUE (email);
ALTER TABLE
university_db=> \d students
                       Table "public.students"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(150) |           |          |
 dob        | date                   |           |          |
Indexes:
    "unique_email" UNIQUE CONSTRAINT, btree (email)


university_db=> ALTER TABLE students RENAME TO learners;
ALTER TABLE
university_db=> \d students
Did not find any relation named "students".
university_db=> \d learners
                       Table "public.learners"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(150) |           |          |
 dob        | date                   |           |          |
Indexes:
    "unique_email" UNIQUE CONSTRAINT, btree (email)


university_db=> ALTER TABLE learners RENAME TO students; -- Change it back
ALTER TABLE
university_db=> \d students
                       Table "public.students"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(150) |           |          |
 dob        | date                   |           |          |
Indexes:
    "unique_email" UNIQUE CONSTRAINT, btree (email)


university_db=> \d learners
Did not find any relation named "learners".
university_db=> DROP TABLE IF EXISTS students;
DROP TABLE
university_db=> \d students
Did not find any relation named "students".
university_db=> -- Re-create students table if previously dropped in activity
university_db=> CREATE TABLE students (
university_db(>     student_id INTEGER,
university_db(>     first_name VARCHAR(50),
university_db(>     last_name VARCHAR(50),
university_db(>     email VARCHAR(100),
university_db(>     dob DATE
university_db(> );
CREATE TABLE
university_db=>
university_db=> -- insert your best frnd names
university_db=>
university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob)
university_db-> VALUES (1, 'Alice', 'Smith', 'alice.smith@example.com', '2003-05-15');
INSERT 0 1
university_db=>
university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob)
university_db-> VALUES (2, 'Bob', 'Johnson', 'bob.johnson@example.com', '2002-08-22'),
university_db->        (3, 'Charlie', 'Brown', 'charlie.brown@example.com', '2003-01-10');
INSERT 0 2
university_db=> \d students
                       Table "public.students"
   Column   |          Type          | Collation | Nullable | Default
------------+------------------------+-----------+----------+---------
 student_id | integer                |           |          |
 first_name | character varying(50)  |           |          |
 last_name  | character varying(50)  |           |          |
 email      | character varying(100) |           |          |
 dob        | date                   |           |          |


university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(3 rows)


university_db=> SELECT * FROM students WHERE student_id = 1;
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com | 2003-05-15
(1 row)


university_db=>
university_db=> SELECT first_name, last_name FROM students WHERE last_name = 'Smith';
 first_name | last_name
------------+-----------
 Alice      | Smith
(1 row)


university_db=>
university_db=> SELECT * FROM students WHERE dob >= '2003-01-01'; -- Students born in or after 2003
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(2 rows)


university_db=>
university_db=> SELECT * FROM students WHERE dob BETWEEN '2002-01-01' AND '2002-12-31'; -- Born in 2002
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          2 | Bob        | Johnson   | bob.johnson@example.com | 2002-08-22
(1 row)


university_db=>
university_db=> SELECT * FROM students WHERE first_name LIKE 'A%';
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com | 2003-05-15
(1 row)


university_db=>
university_db=> SELECT * FROM students WHERE email ILIKE '%.com'; -- Case-insensitive ends with .com
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(3 rows)


university_db=>
university_db=> SELECT * FROM students WHERE first_name = 'Alice' AND last_name = 'Smith';
 student_id | first_name | last_name |          email          |    dob
------------+------------+-----------+-------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com | 2003-05-15
(1 row)


university_db=>
university_db=> SELECT * FROM students WHERE student_id = 1 OR student_id = 3;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(2 rows)


university_db=>
university_db=> SELECT * FROM students WHERE student_id IN (1, 3, 5);
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(2 rows)


university_db=> -- Add a student with a NULL email to test IS NULL
university_db=>
university_db=> INSERT INTO students (student_id, first_name, last_name, dob) VALUES (4, 'Diana', 'Prince', '2001-11-01');
INSERT 0 1
university_db=>
university_db=> SELECT * FROM students WHERE email IS NULL;
 student_id | first_name | last_name | email |    dob
------------+------------+-----------+-------+------------
          4 | Diana      | Prince    |       | 2001-11-01
(1 row)


university_db=>
university_db=> SELECT * FROM students WHERE email IS NOT NULL;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15
          2 | Bob        | Johnson   | bob.johnson@example.com   | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
(3 rows)


university_db=> -- Correct Alice Smith's email
university_db=> UPDATE students
university_db-> SET email = 'alices@example.net'
university_db-> WHERE student_id = 1;
UPDATE 1
university_db=>
university_db=> -- Change multiple columns for Bob Johnson
university_db=> UPDATE students
university_db-> SET first_name = 'Robert', email = 'robert.j@example.com'
university_db-> WHERE student_id = 2;
UPDATE 1
university_db=>
university_db=> SELECT * FROM students WHERE student_id IN (1,2); -- Verify changes
 student_id | first_name | last_name |        email         |    dob
------------+------------+-----------+----------------------+------------
          1 | Alice      | Smith     | alices@example.net   | 2003-05-15
          2 | Robert     | Johnson   | robert.j@example.com | 2002-08-22
(2 rows)


university_db=> -- Add a temporary student to delete
university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob)
university_db-> VALUES (99, 'Temp', 'User', 'temp@example.com', '2000-01-01');
INSERT 0 1
university_db=> SELECT * FROM students WHERE student_id = 99; -- Confirm it's there
 student_id | first_name | last_name |      email       |    dob
------------+------------+-----------+------------------+------------
         99 | Temp       | User      | temp@example.com | 2000-01-01
(1 row)


university_db=>
university_db=> DELETE FROM students WHERE student_id = 99;
DELETE 1
university_db=> SELECT * FROM students WHERE student_id = 99; -- Confirm it's gone
 student_id | first_name | last_name | email | dob
------------+------------+-----------+-------+-----
(0 rows)


university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
(4 rows)


university_db=> SELECT * FROM students ORDER BY last_name ASC; -- Sort by last name A-Z
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          4 | Diana      | Prince    |                           | 2001-11-01
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
(4 rows)


university_db=> SELECT * FROM students ORDER BY dob DESC; -- Sort by date of birth, newest (youngest) first
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          4 | Diana      | Prince    |                           | 2001-11-01
(4 rows)


university_db=> SELECT * FROM students ORDER BY last_name, first_name; -- Sort by last name, then by first name for ties
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          4 | Diana      | Prince    |                           | 2001-11-01
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
(4 rows)


university_db=> SELECT * FROM students ORDER BY student_id ASC; -- Sort by student_id
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
(4 rows)


university_db=> SELECT * FROM students ORDER BY dob DESC LIMIT 3; -- Get the 3 youngest students
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
(3 rows)


university_db=> SELECT * FROM students ORDER BY student_id LIMIT 2 OFFSET 2; -- Get 3rd and 4th students by ID (skip first 2, take next 2)
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
(2 rows)


university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
(4 rows)


university_db=> INSERT INTO students (student_id, first_name, last_name, email, dob)
university_db-> VALUES (5, 'Eve', 'Smith', 'eve.smith@example.com', '2004-07-01');
INSERT 0 1
university_db=>
university_db=> SELECT last_name FROM students; -- Shows all last names, including duplicates
 last_name
-----------
 Brown
 Prince
 Smith
 Johnson
 Smith
(5 rows)


university_db=> SELECT DISTINCT last_name FROM students ORDER BY last_name; -- Shows unique last names
 last_name
-----------
 Brown
 Johnson
 Prince
 Smith
(4 rows)


university_db=> SELECT DISTINCT last_name, first_name FROM students; -- Distinct combinations of last_name and first_name
 last_name | first_name
-----------+------------
 Prince    | Diana
 Smith     | Eve
 Smith     | Alice
 Johnson   | Robert
 Brown     | Charlie
(5 rows)


university_db=> SELECT COUNT(*) AS total_students FROM students;
 total_students
----------------
              5
(1 row)


university_db=> SELECT COUNT(email) AS students_with_email FROM students;
 students_with_email
---------------------
                   4
(1 row)


university_db=> SELECT COUNT(DISTINCT last_name) AS unique_last_names FROM students;
 unique_last_names
-------------------
                 4
(1 row)


university_db=> SELECT MIN(dob) AS oldest_student_dob FROM students;
 oldest_student_dob
--------------------
 2001-11-01
(1 row)


university_db=> SELECT MAX(dob) AS youngest_student_dob FROM students;
 youngest_student_dob
----------------------
 2004-07-01
(1 row)


university_db=> -- Assuming we add a numeric column like 'tuition_paid'
university_db=> -- ALTER TABLE students ADD COLUMN tuition_paid NUMERIC(8,2);
university_db=> -- UPDATE students SET tuition_paid = 5000 WHERE student_id = 1;
university_db=> -- UPDATE students SET tuition_paid = 5500 WHERE student_id = 2;
university_db=> -- SELECT SUM(tuition_paid) AS total_tuition, AVG(tuition_paid) AS avg_tuition FROM students;
university_db=> SELECT * FROM students WHERE student_id > 2;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          5 | Eve        | Smith     | eve.smith@example.com     | 2004-07-01
(3 rows)


university_db=> SELECT * FROM students WHERE student_id <> 1;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          5 | Eve        | Smith     | eve.smith@example.com     | 2004-07-01
(4 rows)


university_db=> SELECT * FROM students WHERE student_id != 1;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          5 | Eve        | Smith     | eve.smith@example.com     | 2004-07-01
(4 rows)


university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob
------------+------------+-----------+---------------------------+------------
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10
          4 | Diana      | Prince    |                           | 2001-11-01
          1 | Alice      | Smith     | alices@example.net        | 2003-05-15
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22
          5 | Eve        | Smith     | eve.smith@example.com     | 2004-07-01
(5 rows)


university_db=> -- Count students for each last name
university_db=> SELECT last_name, COUNT(*) AS number_of_students
university_db-> FROM students
university_db-> GROUP BY last_name
university_db-> ORDER BY number_of_students DESC;
 last_name | number_of_students
-----------+--------------------
 Smith     |                  2
 Johnson   |                  1
 Prince    |                  1
 Brown     |                  1
(4 rows)


university_db=>
university_db=> -- Count students for each last name, only for last names with more than one student
university_db=> SELECT last_name, COUNT(*) AS number_of_students
university_db-> FROM students
university_db-> GROUP BY last_name
university_db-> HAVING COUNT(*) > 1
university_db-> ORDER BY number_of_students DESC;
 last_name | number_of_students
-----------+--------------------
 Smith     |                  2
(1 row)


university_db=>
university_db=> -- PostgreSQL specific way to get year from date: EXTRACT(YEAR FROM date_column)
university_db=> SELECT EXTRACT(YEAR FROM dob) AS birth_year, COUNT(*) AS students_in_year
university_db-> FROM students
university_db-> WHERE dob IS NOT NULL
university_db-> GROUP BY birth_year
university_db-> ORDER BY birth_year;
 birth_year | students_in_year
------------+------------------
       2001 |                1
       2002 |                1
       2003 |                2
       2004 |                1
(4 rows)


university_db=> DROP TABLE IF EXISTS students; -- Start fresh for constraint examples
DROP TABLE
university_db=> CREATE TABLE students (
university_db(>     student_id INTEGER,
university_db(>     first_name VARCHAR(50) NOT NULL, -- first_name cannot be NULL
university_db(>     last_name VARCHAR(50) NOT NULL,
university_db(>     email VARCHAR(100) UNIQUE,      -- email must be unique (and can be NULL by default for UNIQUE)
university_db(>     dob DATE,
university_db(>     enrollment_status VARCHAR(10) CHECK (enrollment_status IN ('enrolled', 'graduated', 'dropped'))
university_db(> );
CREATE TABLE
university_db=> slect * from students;
ERROR:  syntax error at or near "slect"
LINE 1: slect * from students;
        ^
university_db=> select * from students;
 student_id | first_name | last_name | email | dob | enrollment_status
------------+------------+-----------+-------+-----+-------------------
(0 rows)


university_db=> DROP TABLE IF EXISTS students; -- Start fresh
DROP TABLE
university_db=>
university_db=> CREATE TABLE students (
university_db(>     student_id SERIAL PRIMARY KEY,  -- student_id is now PK, auto-incrementing, NOT NULL
university_db(>     first_name VARCHAR(50) NOT NULL,
university_db(>     last_name VARCHAR(50) NOT NULL,
university_db(>     email VARCHAR(100) UNIQUE,      -- Email should be unique; can be NULL unless NOT NULL is added
university_db(>     dob DATE,
university_db(>     enrollment_status VARCHAR(20) CHECK (enrollment_status IN ('enrolled', 'graduated', 'dropped', 'pending'))
university_db(> );
CREATE TABLE
university_db=>
university_db=> -- Insert data (student_id will be auto-generated)
university_db=> INSERT INTO students (first_name, last_name, email, dob, enrollment_status)
university_db-> VALUES ('Alice', 'Smith', 'alice.smith@example.com', '2003-05-15', 'enrolled');
INSERT 0 1
university_db=> INSERT INTO students (first_name, last_name, email, dob, enrollment_status)
university_db-> VALUES ('Robert', 'Johnson', 'robert.j@example.com', '2002-08-22', 'enrolled');
INSERT 0 1
university_db=> INSERT INTO students (first_name, last_name, email, dob, enrollment_status)
university_db-> VALUES ('Charlie', 'Brown', 'charlie.brown@example.com', '2003-01-10', 'pending');
INSERT 0 1
university_db=>
university_db=> SELECT * FROM students; -- Note the student_id values (1, 2, 3...)
 student_id | first_name | last_name |           email           |    dob     | enrollment_status
------------+------------+-----------+---------------------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10 | pending
(3 rows)


university_db=> CREATE TABLE courses (
university_db(>     course_id SERIAL PRIMARY KEY,
university_db(>     course_name VARCHAR(100) NOT NULL UNIQUE,
university_db(>     credits INTEGER CHECK (credits > 0 AND credits < 10) -- Example of CHECK
university_db(> );
CREATE TABLE
university_db=>
university_db=> INSERT INTO courses (course_name, credits) VALUES ('Introduction to SQL', 3);
INSERT 0 1
university_db=> INSERT INTO courses (course_name, credits) VALUES ('Database Design', 4);
INSERT 0 1
university_db=> INSERT INTO courses (course_name, credits) VALUES ('Web Development', 3);
INSERT 0 1
university_db=> INSERT INTO courses (course_name, credits) VALUES ('Data Structures', 4);
INSERT 0 1
university_db=> SELECT * FROM courses;
 course_id |     course_name     | credits
-----------+---------------------+---------
         1 | Introduction to SQL |       3
         2 | Database Design     |       4
         3 | Web Development     |       3
         4 | Data Structures     |       4
(4 rows)


university_db=> INSERT INTO students (first_name, last_name, email, dob, enrollment_status)
university_db-> VALUES ('keelie', 'kk', 'keelie.kk@example.com', '2003-08-08', 'enrolled')
university_db-> ;
INSERT 0 1
university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob     | enrollment_status
------------+------------+-----------+---------------------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10 | pending
          4 | keelie     | kk        | keelie.kk@example.com     | 2003-08-08 | enrolled
(4 rows)


university_db=> WHERE first_name = keelie UPDATE INTO students enrollment_status = unenrolled;
ERROR:  syntax error at or near "WHERE"
LINE 1: WHERE first_name = keelie UPDATE INTO students enrollment_st...
        ^
university_db=> SELECT * FROM students WHERE first_name = keelie; UPDATE INTO students enrollment_status = unenrolled;
ERROR:  column "keelie" does not exist
LINE 1: SELECT * FROM students WHERE first_name = keelie;
                                                  ^
ERROR:  syntax error at or near "INTO"
LINE 1: UPDATE INTO students enrollment_status = unenrolled;
               ^
university_db=> UPDATE students
university_db-> SET enrollment_status = 'unenrolled'
university_db-> WHERE first_name = 'Keelie';
UPDATE 0
university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob     | enrollment_status
------------+------------+-----------+---------------------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10 | pending
          4 | keelie     | kk        | keelie.kk@example.com     | 2003-08-08 | enrolled
(4 rows)


university_db=> UPDATE students
university_db-> SET enrollment_status = 'graduated'
university_db-> WHERE first_name = 'Keelie';
UPDATE 0
university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob     | enrollment_status
------------+------------+-----------+---------------------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10 | pending
          4 | keelie     | kk        | keelie.kk@example.com     | 2003-08-08 | enrolled
(4 rows)


university_db=> UPDATE students
university_db-> SET enrollment_status = 'dropped'
university_db-> WHERE first_name = 'keelie';
UPDATE 1
university_db=> SELECT * FROM students;
 student_id | first_name | last_name |           email           |    dob     | enrollment_status
------------+------------+-----------+---------------------------+------------+-------------------
          1 | Alice      | Smith     | alice.smith@example.com   | 2003-05-15 | enrolled
          2 | Robert     | Johnson   | robert.j@example.com      | 2002-08-22 | enrolled
          3 | Charlie    | Brown     | charlie.brown@example.com | 2003-01-10 | pending
          4 | keelie     | kk        | keelie.kk@example.com     | 2003-08-08 | dropped
(4 rows)


university_db=> select count(distinct last_name) as dln in students;
ERROR:  syntax error at or near "in"
LINE 1: select count(distinct last_name) as dln in students;
                                                ^
university_db=> select count(distinct last_name) as dln from students;
 dln
-----
   4
(1 row)


university_db=> ALTER TABLE students
university_db-> ADD PRIMARY KEY (enrollment_status);
ERROR:  multiple primary keys for table "students" are not allowed
university_db=>
```
