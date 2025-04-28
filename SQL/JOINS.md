In PostgreSQL (and SQL in general), **joins** are used to combine rows from two or more tables based on a related column between them. Here's an explanation of **inner join**, **self join**, and **outer join**, along with examples:

---

### **1. INNER JOIN**
An **inner join** returns only the rows that have matching values in both tables. If there is no match, the row is excluded from the result.

#### Syntax:
```sql
SELECT columns
FROM table1
INNER JOIN table2
ON table1.column = table2.column;
```

#### Example:
Suppose you have two tables:
- `employees`: `id`, `name`, `department_id`
- `departments`: `department_id`, `department_name`

To get the names of employees along with their department names:
```sql
SELECT employees.name, departments.department_name
FROM employees
INNER JOIN departments
ON employees.department_id = departments.department_id;
```

#### Output:
| name      | department_name |
|-----------|-----------------|
| John Doe  | HR              |
| Jane Smith| Engineering     |

---

### **2. SELF JOIN**
A **self join** is a join where a table is joined with itself. It is useful when you need to compare rows within the same table.

#### Syntax:
```sql
SELECT t1.columns, t2.columns
FROM table t1
INNER JOIN table t2
ON t1.column = t2.column;
```

#### Example:
Suppose you have a `employees` table with `id`, `name`, and `manager_id` (where `manager_id` refers to the `id` of another employee in the same table).

To get the names of employees and their managers:
```sql
SELECT e1.name AS employee_name, e2.name AS manager_name
FROM employees e1
INNER JOIN employees e2
ON e1.manager_id = e2.id;
```

#### Output:
| employee_name | manager_name |
|---------------|--------------|
| John Doe      | Jane Smith   |
| Alice Brown   | Jane Smith   |

---

### **3. OUTER JOIN**
An **outer join** returns all rows from one table and the matching rows from the other table. If there is no match, the result will include `NULL` for the columns from the table without a match.

There are three types of outer joins:
- **LEFT OUTER JOIN** (or **LEFT JOIN**): Returns all rows from the left table and matching rows from the right table.
- **RIGHT OUTER JOIN** (or **RIGHT JOIN**): Returns all rows from the right table and matching rows from the left table.
- **FULL OUTER JOIN** (or **FULL JOIN**): Returns all rows when there is a match in either the left or right table.

#### Syntax:
```sql
-- LEFT JOIN
SELECT columns
FROM table1
LEFT JOIN table2
ON table1.column = table2.column;

-- RIGHT JOIN
SELECT columns
FROM table1
RIGHT JOIN table2
ON table1.column = table2.column;

-- FULL JOIN
SELECT columns
FROM table1
FULL JOIN table2
ON table1.column = table2.column;
```

#### Example:
Using the same `employees` and `departments` tables:

1. **LEFT JOIN** (Get all employees and their departments, even if some employees don't belong to a department):
   ```sql
   SELECT employees.name, departments.department_name
   FROM employees
   LEFT JOIN departments
   ON employees.department_id = departments.department_id;
   ```

   | name        | department_name |
   |-------------|-----------------|
   | John Doe    | HR              |
   | Jane Smith  | Engineering     |
   | Alice Brown | NULL            |

2. **RIGHT JOIN** (Get all departments and their employees, even if some departments have no employees):
   ```sql
   SELECT employees.name, departments.department_name
   FROM employees
   RIGHT JOIN departments
   ON employees.department_id = departments.department_id;
   ```

   | name      | department_name |
   |-----------|-----------------|
   | John Doe  | HR              |
   | Jane Smith| Engineering     |
   | NULL      | Marketing       |

3. **FULL JOIN** (Get all employees and all departments, with `NULL` where there is no match):
   ```sql
   SELECT employees.name, departments.department_name
   FROM employees
   FULL JOIN departments
   ON employees.department_id = departments.department_id;
   ```

   | name        | department_name |
   |-------------|-----------------|
   | John Doe    | HR              |
   | Jane Smith  | Engineering     |
   | Alice Brown | NULL            |
   | NULL        | Marketing       |

---

### **Key Differences Between Joins**
| Join Type      | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| **INNER JOIN** | Returns only matching rows from both tables.                                |
| **SELF JOIN**  | Joins a table with itself (useful for hierarchical or comparative queries). |
| **LEFT JOIN**  | Returns all rows from the left table and matching rows from the right table. |
| **RIGHT JOIN** | Returns all rows from the right table and matching rows from the left table. |
| **FULL JOIN**  | Returns all rows when there is a match in either the left or right table.    |

---

### **When to Use Each Join**
- Use **INNER JOIN** when you only want matching rows.
- Use **SELF JOIN** when comparing rows within the same table.
- Use **LEFT JOIN** when you want all rows from the left table, even if there are no matches in the right table.
- Use **RIGHT JOIN** when you want all rows from the right table, even if there are no matches in the left table.
- Use **FULL JOIN** when you want all rows from both tables, with `NULL` for non-matching rows.

Let me know if you need further clarification or additional examples!