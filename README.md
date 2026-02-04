# sql-mastering-joins
"Advanced data merging techniques in PostgreSQL. Practical implementation of INNER, LEFT, RIGHT, and FULL joins to analyze relationships between employees and departments."
# 🤝 Mastering SQL Joins

Understanding Joins is the key to turning raw data into meaningful reports. This project demonstrates the four primary ways to combine tables in PostgreSQL.



## 📋 Join Comparison Table

| Join Type | Result | Business Use Case |
| :--- | :--- | :--- |
| **INNER JOIN** | Matches found in both tables. | Showing a list of active employees and their offices. |
| **LEFT JOIN** | All from Left table + matches from Right. | Identifying employees who haven't been assigned a team yet. |
| **RIGHT JOIN** | All from Right table + matches from Left. | Auditing departments to see which ones are currently empty. |
| **FULL JOIN** | Everything from both tables. | A complete audit of all staff and all departments. |

## 🧠 Key Learnings
- **Table Aliases:** Using `e` for employees and `d` for departments makes queries much shorter and easier to read.
- **PostgreSQL vs MySQL:** PostgreSQL supports `FULL OUTER JOIN` natively, whereas MySQL requires a `UNION` of a Left and Right join to achieve the same result.
- **Handling NULLs:** When a join doesn't find a match, the columns from the missing side are filled with `NULL`.

## 🛠️ How to Test
1. Run the `mastering_joins.sql` script in **pgAdmin**.
2. Compare the number of rows returned by the **INNER JOIN** (only matches) vs the **FULL OUTER JOIN** (everything).

-- ==========================================================
-- PROJECT: Mastering Table Joins (Task 8)
-- CONCEPTS: INNER, LEFT, RIGHT, FULL OUTER JOIN
-- ==========================================================

-- 1. SETUP DATA FOR JOIN PRACTICE
-- Let's ensure we have:
-- - Some employees with departments
-- - One employee with NO department (null dept_id)
-- - One department with NO employees

INSERT INTO departments (dept_name, location) VALUES ('R&D', 'Tokyo'); -- Empty Dept

INSERT INTO employees (first_name, last_name, dept_id, salary) 
VALUES ('Freelancer', 'Dave', NULL, 50000); -- Employee with no Dept

-- 2. INNER JOIN: The "Perfect Match"
-- Returns only employees who belong to a department.
SELECT 
    e.first_name, 
    e.last_name, 
    d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.dept_id;

-- 3. LEFT JOIN: "All Employees"
-- Returns all employees, even if they don't have a department (like Dave).
SELECT 
    e.first_name, 
    e.last_name, 
    d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.dept_id;

-- 4. RIGHT JOIN: "All Departments"
-- Returns all departments, even if they have no employees (like R&D).
SELECT 
    e.first_name, 
    d.dept_name
FROM employees e
RIGHT JOIN departments d ON e.dept_id = d.dept_id;

-- 5. FULL OUTER JOIN: "The Complete Picture"
-- Returns everything from both tables, filling in NULLs where matches are missing.
SELECT 
    e.first_name, 
    d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.dept_id;

-- 6. BUSINESS CASE: Find "Orphan" Records
-- Identify departments that currently have no staff assigned.
SELECT d.dept_name
FROM departments d
LEFT JOIN employees e ON d.dept_id = e.dept_id
WHERE e.emp_id IS NULL;
