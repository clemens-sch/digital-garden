#DataBases 

---
## # Introduction

In this node I explain some basic keywords in combination with SQL

---
## Listed Keywords
### # VIEW
### # UNION
### # WITH
---
## # VIEW

refers to a virtual table based on one or more tables. 
A view allows you to store complex queries and retrieve them as if they were a single table.

#### # Example of a simple VIEW

Assuming you have a table `employees` with columns `employee_id`, `name`, and `salary`, you could create a simple view to display only specific information:

```sql
CREATE VIEW EmployeeInfo AS
SELECT employee_id, name
FROM employees;
```

Now, you can use the `EmployeeInfo` view to display only the `employee_id` and `name` columns instead of searching through the entire `employees` table.

```sql
SELECT * FROM EmployeeInfo
```

---
## # UNION

is used to combine the results of two or more SELECT statements and remove duplicate rows.

#### # Example of a UNION Statement

Let's consider an example with two tables, `students_math` and `students_physics`, both with columns `student_id` and `name`. We want to create a list of all unique students who have either studied math or physics:

```sql
SELECT student_id, name FROM students_math
UNION
-- UNION ALL - for distinct entries 
SELECT student_id, name FROM students_physics;
```

giving us a list of distinct students,
who have studied either math or physics. If a student has studied both, duplicates are removed, and the result set contains a unique list of students across the two tables.

---
## # WITH

allows you to define a named query result temporarily and then use it as if it were a table in the rest of your SQL statement. It's a way to break down complex queries into more manageable parts for better readability and maintenance.

#### # Example of a WITH Statement

Let's say you have a table `employees` with columns `employee_id`, `name`, and `salary`, and you want to find the average salary and display employees with salaries above that average:

```sql
WITH AverageSalary AS (
  SELECT AVG(salary) AS avg_salary
  FROM employees
)

SELECT employee_id, name, salary
FROM employees
WHERE salary > (SELECT avg_salary FROM AverageSalary);
```

In this example, the `WITH` statement creates a CTE named `AverageSalary`, which calculates the average salary from the `employees` table. The subsequent `SELECT` statement then uses this CTE to filter and display only those employees whose salary is above the calculated average.

---