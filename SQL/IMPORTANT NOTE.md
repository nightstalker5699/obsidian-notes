# COALESCE
`COALESCE` in PostgreSQL returns the first non-`NULL` value from a list of expressions. If all values are `NULL`, it returns `NULL`.

### Syntax:

```sql
COALESCE(expression_1, expression_2, ..., expression_n)
```

### Example:

```sql
SELECT COALESCE(NULL, NULL, 'Hello', 'World');
-- Returns 'Hello'
```

### Use Cases:

- Replace `NULL` with a default value:
    
    ```sql
    SELECT COALESCE(discount, 0) FROM orders;
    ```
    
- Return the first non-`NULL` column value:
    
    ```sql
    SELECT COALESCE(first_name, middle_name, last_name) FROM users;
    ```
    

It helps in handling `NULL` values in queries.


# CASTING
### **Casting in SQL (`CAST` & `CONVERT`)**

**Casting** in SQL means converting data from one type to another (e.g., string to integer, date to string). Different databases provide functions for this.

---

## **1. Using `CAST()` (Standard SQL)**

Syntax:

```sql
CAST(expression AS target_data_type)
```

### **Examples:**

#### **Convert a String to an Integer**

```sql
SELECT CAST('123' AS INT);  -- Output: 123
```

#### **Convert an Integer to a String**

```sql
SELECT CAST(123 AS VARCHAR(10));  -- Output: '123'
```

#### **Convert a String to a Date**

```sql
SELECT CAST('2024-03-18' AS DATE);
```


---

## **2. Implicit vs. Explicit Casting**

- **Implicit Casting:** SQL automatically converts compatible types.
    
    ```sql
    SELECT 100 + '50';  -- Output: 150 (string '50' is converted to an integer)
    ```
    
- **Explicit Casting:** Use `CAST()` or `CONVERT()` when SQL does not auto-convert.

---

## **3. Type-Specific Casting**

### **MySQL: `CAST()` & `CONVERT()`**

```sql
SELECT CAST('3.14' AS DECIMAL(5,2));  -- Output: 3.14
SELECT CONVERT('2024-03-18', DATE);   -- Output: 2024-03-18
```

### **PostgreSQL: `::` Shortcut**

```sql
SELECT '123'::INT;  -- Output: 123
SELECT NOW()::DATE;  -- Output: 2024-03-18
```

---

## **4. Handling Errors in Casting**

- If conversion is **impossible**, SQL throws an error.
- Example:
    
    ```sql
    SELECT CAST('abc' AS INT);  -- ERROR: Cannot convert 'abc' to INT
    ```
    
- **Fix:** Use `TRY_CAST()` (SQL Server) or `NULLIF()`
    
    ```sql
    SELECT TRY_CAST('abc' AS INT);  -- Output: NULL (instead of error)
    ```
    

---

### **Key Takeaways**

✔ `CAST()` – Standard SQL, works in all databases.  
✔ PostgreSQL allows `::` shorthand (`'123'::INT`).  
✔ Use `TRY_CAST()` (SQL Server) to avoid errors.

# DATE FORMAT
### **PostgreSQL and ISO 8601 Date Format**

PostgreSQL **natively supports** the **ISO 8601** standard for date and time formatting. This format is **YYYY-MM-DDTHH:MI:SS+TZ** (e.g., `2024-03-18T15:30:00+00:00`).

---

## **1. Getting Current Timestamp in ISO 8601 Format**

```sql
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD"T"HH24:MI:SSOF');
```

✔ Example Output: `2024-03-18T15:30:00+00:00`

---

## **2. Using PostgreSQL’s Default ISO 8601 Format**

PostgreSQL **by default** uses ISO 8601 for `TIMESTAMP WITH TIME ZONE`:

```sql
SELECT NOW();
```

✔ Example Output: `2024-03-18 15:30:00+00`

---

## **3. Enforcing ISO 8601 in Output**

```sql
SET datestyle = 'ISO, YMD';
SELECT NOW();
```

✔ Ensures **YYYY-MM-DD** format.

---

## **4. Converting to ISO 8601 Manually**

If you need strict ISO formatting:

```sql
SELECT TO_CHAR(NOW(), 'YYYY-MM-DD"T"HH24:MI:SSOF');
```

✔ Example Output: `2024-03-18T15:30:00+00:00`

---

## **5. Parsing an ISO 8601 String into a `TIMESTAMP`**

```sql
SELECT '2024-03-18T15:30:00+00:00'::TIMESTAMPTZ;
```

✔ Converts **ISO 8601 string** into a PostgreSQL `TIMESTAMP WITH TIME ZONE`.

---

### **Key Takeaways**

- ✅ PostgreSQL **supports ISO 8601 by default** for timestamps.
- ✅ Use `SET datestyle = 'ISO, YMD';` to enforce formatting.
- ✅ Use `TO_CHAR(NOW(), 'YYYY-MM-DD"T"HH24:MI:SSOF')` for strict ISO output.
- ✅ Cast strings (`'2024-03-18T15:30:00+00:00'::TIMESTAMPTZ`) to convert.

Would you like an example for **time zone conversions**? 🚀