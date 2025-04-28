### **Pattern Searching in SQL**

Pattern searching allows you to **find text that matches a specific pattern** within a column. This is useful for **partial matches, wildcard searches, and complex text filtering**.

---

## **1. Methods of Pattern Searching**

### **A. `LIKE` (Basic Pattern Matching)**

- Uses **wildcards** (`%`, `_`) to match patterns.
- Works in **MySQL, PostgreSQL, SQL Server, and Oracle**.

```sql
SELECT * FROM employees WHERE name LIKE 'A%';  -- Names starting with 'A'
SELECT * FROM employees WHERE name LIKE '%son';  -- Names ending with 'son'
SELECT * FROM employees WHERE name LIKE '_a%';  -- Second letter is 'a'
```

🔹 **Wildcards:**

|Wildcard|Meaning|Example|
|---|---|---|
|`%`|Any number of characters|`'A%'` → _Alice, Adam_|
|`_`|Exactly one character|`'A_'` → _Al, An_|
|`%text%`|Contains text anywhere|`'%son%'` → _Johnson, Jason_|

---

### **B. `ILIKE` (Case-Insensitive LIKE – PostgreSQL)**

- Works **like `LIKE` but ignores case**.

```sql
SELECT * FROM employees WHERE name ILIKE 'john%';  
```

✔ Matches **John, john, JOHN**.

---

### **C. `SIMILAR TO` (PostgreSQL & SQL Standard)**

- Works like `LIKE`, but **supports regular expressions**.

```sql
SELECT * FROM employees WHERE name SIMILAR TO 'J(oh|ac)%';  
```

✔ Matches **John, Jack**.

---

### **D. `REGEXP` / `RLIKE` (Regular Expressions – MySQL, PostgreSQL, SQL Server)**

- More advanced than `LIKE`.

```sql
SELECT * FROM employees WHERE name REGEXP '^A.*son$';  
```

✔ Matches **names starting with 'A' and ending with 'son'**, e.g., **"Anderson"**.

---

## **2. When to Use Each?**

|Method|Supports Case-Insensitive?|Supports Complex Patterns?|Database Support|
|---|---|---|---|
|`LIKE`|❌ No|❌ No|MySQL, PostgreSQL, SQL Server, Oracle|
|`ILIKE`|✅ Yes|❌ No|PostgreSQL|
|`SIMILAR TO`|❌ No|✅ Yes (RegEx-like)|PostgreSQL|
|`REGEXP` / `RLIKE`|✅ Yes|✅ Yes (Full RegEx)|MySQL, PostgreSQL|

---

## **3. Best Practice Tips**

- ✅ **Use `LIKE` for simple searches** (e.g., starts with, ends with).
- ✅ **Use `REGEXP` for advanced patterns** (e.g., matching multiple words).
- ⚠ **Avoid `LIKE '%text%'` on large tables** (it slows queries).
- ⚡ **Use FULLTEXT indexing for big text searches** (in MySQL, PostgreSQL).

---
