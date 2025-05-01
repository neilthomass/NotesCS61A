---
title: "SQL"
---

# Introduction to SQL

SQL (Structured Query Language) is a powerful language used for managing and manipulating relational databases. It allows us to create, read, update, and delete data in a structured way.

## Basic Concepts

### Tables
Tables are the fundamental structure in SQL databases. They consist of:
- Columns (attributes)
- Rows (records)
- Primary keys (unique identifiers)

Example table structure:
```sql
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    major TEXT,
    gpa FLOAT
);
```

### Basic Operations

#### SELECT
The most common operation to retrieve data:
```sql
-- Select all columns
SELECT * FROM students;

-- Select specific columns
SELECT name, major FROM students;

-- Select with conditions
SELECT * FROM students WHERE gpa > 3.5;
```

#### INSERT
Adding new records:
```sql
-- Insert a single record
INSERT INTO students (name, major, gpa) 
VALUES ('John Doe', 'Computer Science', 3.8);

-- Insert multiple records
INSERT INTO students (name, major, gpa) VALUES 
    ('Jane Smith', 'Mathematics', 3.9),
    ('Bob Johnson', 'Physics', 3.7);
```

#### UPDATE
Modifying existing records:
```sql
-- Update a single record
UPDATE students 
SET gpa = 3.9 
WHERE name = 'John Doe';

-- Update multiple records
UPDATE students 
SET major = 'Computer Science' 
WHERE major = 'CS';
```

#### DELETE
Removing records:
```sql
-- Delete specific records
DELETE FROM students 
WHERE gpa < 2.0;

-- Delete all records
DELETE FROM students;
```

## Advanced Queries

### Joins
Combining data from multiple tables:
```sql
-- Inner join
SELECT students.name, courses.name
FROM students
JOIN enrollments ON students.id = enrollments.student_id
JOIN courses ON enrollments.course_id = courses.id;



### Aggregation
Performing calculations on groups of data:
```sql
-- Count
SELECT COUNT(*) FROM students;

-- Average
SELECT AVG(gpa) FROM students;

-- Group by
SELECT major, AVG(gpa) 
FROM students 
GROUP BY major;
```

### Subqueries
Queries within queries:
```sql
-- Find students with above-average GPA
SELECT name, gpa 
FROM students 
WHERE gpa > (SELECT AVG(gpa) FROM students);
```

## Best Practices

1. **Use meaningful table and column names**
   - Be descriptive and consistent
   - Use underscores for spaces
   - Avoid reserved words

2. **Always specify columns in INSERT statements**
   ```sql
   -- Good
   INSERT INTO students (name, major) VALUES ('John', 'CS');
   
   -- Bad
   INSERT INTO students VALUES ('John', 'CS');
   ```

3. **Use appropriate data types**
   - INTEGER for whole numbers
   - FLOAT for decimal numbers
   - TEXT for strings
   - BOOLEAN for true/false values

4. **Include WHERE clauses in UPDATE and DELETE**
   - Prevents accidental updates/deletes
   - Always test with SELECT first

5. **Use transactions for multiple operations**
   ```sql
   BEGIN TRANSACTION;
   UPDATE accounts SET balance = balance - 100 WHERE id = 1;
   UPDATE accounts SET balance = balance + 100 WHERE id = 2;
   COMMIT;
   ```

## Common Pitfalls

1. **Forgetting WHERE clauses**
   - Can lead to updating/deleting all records
   - Always double-check conditions

2. **Not using indexes**
   - Can cause slow queries on large tables
   - Add indexes on frequently searched columns

3. **Overusing SELECT ***
   - Only select needed columns
   - Improves performance and clarity

4. **Ignoring NULL values**
   - NULL is not the same as 0 or empty string
   - Use IS NULL or IS NOT NULL for comparisons

## Example Database Schema

```sql
-- Students table
CREATE TABLE students (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    major TEXT,
    gpa FLOAT
);

-- Courses table
CREATE TABLE courses (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    department TEXT,
    credits INTEGER
);

-- Enrollments table (junction table)
CREATE TABLE enrollments (
    student_id INTEGER,
    course_id INTEGER,
    grade TEXT,
    FOREIGN KEY (student_id) REFERENCES students(id),
    FOREIGN KEY (course_id) REFERENCES courses(id)
);
```

This schema demonstrates:
- Primary keys
- Foreign keys
- Many-to-many relationships
- Appropriate data types
- Table relationships 