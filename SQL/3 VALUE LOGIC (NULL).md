### **Three-Valued Logic (3VL) in SQL: A Deeper Dive**

SQL uses **three-valued logic (3VL)** to handle `NULL` values. This is different from traditional Boolean logic, which has only **TRUE** and **FALSE**. In SQL, **NULL** represents **missing or unknown data**, leading to a third logical value: **UNKNOWN**.

---

## **1. Understanding NULL in SQL**

- `NULL` means **unknown**, **missing**, or **not applicable**.
- `NULL` **is not** equal to anything, not even to another `NULL`.
- Any arithmetic or logical operation involving `NULL` results in `NULL` (i.e., UNKNOWN).

For example:

```sql
SELECT 10 + NULL;  -- Result: NULL
SELECT 'hello' || NULL;  -- Result: NULL
SELECT NULL = NULL;  -- Result: UNKNOWN (not TRUE or FALSE)
```

---

## **2. Logical Operations with NULL (3VL Tables)**

### **AND (`AND`) Operator**

|A|B|A AND B|
|---|---|---|
|TRUE|TRUE|TRUE|
|TRUE|FALSE|FALSE|
|TRUE|UNKNOWN|UNKNOWN|
|FALSE|FALSE|FALSE|
|FALSE|UNKNOWN|FALSE|
|UNKNOWN|UNKNOWN|UNKNOWN|

👉 **Key takeaway**: If one condition is `FALSE`, the result is `FALSE`, regardless of `UNKNOWN`. Otherwise, `UNKNOWN` spreads.

---

### **OR (`OR`) Operator**

|A|B|A OR B|
|---|---|---|
|TRUE|TRUE|TRUE|
|TRUE|FALSE|TRUE|
|TRUE|UNKNOWN|TRUE|
|FALSE|FALSE|FALSE|
|FALSE|UNKNOWN|UNKNOWN|
|UNKNOWN|UNKNOWN|UNKNOWN|

👉 **Key takeaway**: If one condition is `TRUE`, the result is `TRUE`, regardless of `UNKNOWN`. Otherwise, `UNKNOWN` spreads.

---

### **NOT (`NOT`) Operator**

|A|NOT A|
|---|---|
|TRUE|FALSE|
|FALSE|TRUE|
|UNKNOWN|UNKNOWN|

👉 **Key takeaway**: `NOT UNKNOWN` is still `UNKNOWN`—negation doesn’t resolve uncertainty.

---

## **3. NULL in Comparisons and Filtering**

### **Comparing NULL Values (`=` and `!=` Fail!)**

```sql
SELECT * FROM employees WHERE salary = NULL;  -- Always returns no rows
SELECT * FROM employees WHERE salary != 50000;  -- NULL salaries are ignored
```

👉 **Why?** Because `NULL = NULL` is **UNKNOWN**, so the condition never evaluates to `TRUE`.

**✅ Correct way to check for NULL values:**

```sql
SELECT * FROM employees WHERE salary IS NULL;
SELECT * FROM employees WHERE salary IS NOT NULL;
```

---

## **4. Handling NULL Values Properly**

### **Using `COALESCE()` to Replace NULL**

```sql
SELECT name, COALESCE(salary, 0) AS salary FROM employees;
```

✅ If `salary` is `NULL`, it gets replaced with `0`.

### **Using `CASE` for Custom Handling**

```sql
SELECT 
    name, 
    CASE 
        WHEN salary IS NULL THEN 'Unknown'
        WHEN salary < 50000 THEN 'Low Salary'
        ELSE 'High Salary' 
    END AS salary_category
FROM employees;
```

✅ Categorizes employees based on their salary, treating `NULL` correctly.

---

## **5. NULL in `JOIN` Operations**

```sql
SELECT * FROM orders o 
LEFT JOIN customers c 
ON o.customer_id = c.customer_id;
```

- If there is no matching `customer_id` in `customers`, the `customer_id` column in the result will be **NULL**.
- This happens because `NULL = NULL` is **UNKNOWN**, so the row isn’t matched in an `INNER JOIN`.

### **Fix: Use `IS NULL` to Find Unmatched Rows**

```sql
SELECT * FROM orders 
LEFT JOIN customers ON orders.customer_id = customers.customer_id
WHERE customers.customer_id IS NULL;
```

✅ Finds orders with no matching customer.

---

## **6. NULL in `GROUP BY` and Aggregation**

### **How Aggregation Treats NULL**

- **COUNT(column)**: Ignores `NULL` values.
- **COUNT(*)**: Counts all rows, including those with `NULL`.
- **SUM() / AVG()**: Ignores `NULL`, treating them as missing.

```sql
SELECT 
    COUNT(salary) AS count_salary,  -- Excludes NULLs
    COUNT(*) AS count_rows,  -- Includes all rows
    SUM(salary) AS total_salary,  -- Ignores NULLs
    AVG(salary) AS avg_salary  -- Ignores NULLs
FROM employees;
```

---

## **7. NULL in `HAVING` Clause**

```sql
SELECT department, AVG(salary) AS avg_salary
FROM employees
GROUP BY department
HAVING AVG(salary) > 50000;
```

- Departments where all salaries are `NULL` will **not appear** because `AVG(salary)` is `NULL`, and `NULL > 50000` is `UNKNOWN`.

✅ **Fix: Handle NULLs Properly**

```sql
HAVING COALESCE(AVG(salary), 0) > 50000;
```

---

## **Key Takeaways**

1. **NULL is not equal to anything**, including itself (`NULL = NULL` is `UNKNOWN`).
2. **AND, OR, and NOT behave differently with NULL**, following three-valued logic.
3. **`WHERE` conditions fail on NULL comparisons unless using `IS NULL`**.
4. **Use `COALESCE()` to replace NULL values**.
5. **Aggregations (`COUNT`, `SUM`, `AVG`) ignore NULLs**.
6. **JOINs with NULL require careful handling** (`IS NULL` for unmatched rows).
