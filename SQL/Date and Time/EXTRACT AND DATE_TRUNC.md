In PostgreSQL, both `EXTRACT` and `DATE_TRUNC` are used for working with dates and timestamps, but they serve different purposes:

1. **`EXTRACT`**: Extracts a specific part (e.g., year, month, day) from a date or timestamp and returns it as a numeric value.
2. **`DATE_TRUNC`**: Truncates a date or timestamp to a specified precision (e.g., year, month, day) and returns the truncated value as a timestamp.

Here’s a detailed explanation of both functions with examples:

---

### **`EXTRACT` Function**
Extracts a specific field (e.g., year, month, day) from a date, timestamp, or interval.

#### Syntax:
```sql
EXTRACT(field FROM source)
```

#### Common Fields:
- `YEAR`, `MONTH`, `DAY`
- `HOUR`, `MINUTE`, `SECOND`
- `DOW` (day of week, 0 = Sunday, 6 = Saturday)
- `DOY` (day of year, 1 to 366)
- `QUARTER` (1 to 4)
- `EPOCH` (seconds since 1970-01-01)

#### Examples:
1. Extract the year from a date:
   ```sql
   SELECT EXTRACT(YEAR FROM '2023-10-05'::DATE);
   -- Output: 2023
   ```

2. Extract the month from a timestamp:
   ```sql
   SELECT EXTRACT(MONTH FROM '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 10
   ```

3. Extract the day of the week (DOW):
   ```sql
   SELECT EXTRACT(DOW FROM '2023-10-05'::DATE);
   -- Output: 4 (Thursday)
   ```

4. Extract the epoch (seconds since 1970-01-01):
   ```sql
   SELECT EXTRACT(EPOCH FROM '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 1696516200
   ```

---

### **`DATE_TRUNC` Function**
Truncates a date or timestamp to a specified precision (e.g., year, month, day) and returns the truncated value as a timestamp.

#### Syntax:
```sql
DATE_TRUNC(precision, source)
```

#### Common Precisions:
- `year`, `quarter`, `month`, `week`, `day`
- `hour`, `minute`, `second`

#### Examples:
1. Truncate to the start of the year:
   ```sql
   SELECT DATE_TRUNC('year', '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 2023-01-01 00:00:00
   ```

2. Truncate to the start of the month:
   ```sql
   SELECT DATE_TRUNC('month', '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 2023-10-01 00:00:00
   ```

3. Truncate to the start of the day:
   ```sql
   SELECT DATE_TRUNC('day', '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 2023-10-05 00:00:00
   ```

4. Truncate to the start of the hour:
   ```sql
   SELECT DATE_TRUNC('hour', '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 2023-10-05 14:00:00
   ```

5. Truncate to the start of the week:
   ```sql
   SELECT DATE_TRUNC('week', '2023-10-05 14:30:00'::TIMESTAMP);
   -- Output: 2023-10-02 00:00:00 (assuming the week starts on Monday)
   ```

---

### **Key Differences Between `EXTRACT` and `DATE_TRUNC`**
| Feature                | `EXTRACT`                          | `DATE_TRUNC`                        |
|------------------------|------------------------------------|-------------------------------------|
| **Purpose**            | Extracts a specific part of a date | Truncates a date to a given precision |
| **Return Type**        | Numeric value                     | Timestamp                          |
| **Example Use Case**   | Get the year, month, or day       | Round a timestamp to the start of the month, day, etc. |

---

### **Combining `EXTRACT` and `DATE_TRUNC`**
You can use both functions together for more advanced queries. For example:
1. Extract the month from a truncated timestamp:
   ```sql
   SELECT EXTRACT(MONTH FROM DATE_TRUNC('month', '2023-10-05 14:30:00'::TIMESTAMP));
   -- Output: 10
   ```

2. Count the number of records per month:
   ```sql
   SELECT DATE_TRUNC('month', created_at) AS month, COUNT(*)
   FROM orders
   GROUP BY month
   ORDER BY month;
   ```

---

Let me know if you need further clarification or additional examples!