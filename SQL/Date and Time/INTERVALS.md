In PostgreSQL, **`INTERVAL`** is a data type used to represent a span of time, such as "2 days" or "3 hours and 30 minutes." It is often used in date and time calculations, such as adding or subtracting time from a date or timestamp.

Here’s how you can work with `INTERVAL` in PostgreSQL, including its use with `EXTRACT` and `DATE_TRUNC`:

---

### **1. Creating an Interval**
You can create an interval using the `INTERVAL` keyword followed by a string representing the time span.

#### Syntax:
```sql
INTERVAL 'quantity unit [quantity unit ...]'
```

#### Examples:
1. A simple interval of 5 days:
   ```sql
   SELECT INTERVAL '5 days';
   -- Output: 5 days
   ```

2. A complex interval of 2 years, 3 months, and 10 days:
   ```sql
   SELECT INTERVAL '2 years 3 months 10 days';
   -- Output: 2 years 3 mons 10 days
   ```

3. An interval of 3 hours and 30 minutes:
   ```sql
   SELECT INTERVAL '3 hours 30 minutes';
   -- Output: 03:30:00
   ```

---

### **2. Using Interval in Date/Time Calculations**
You can add or subtract intervals from dates or timestamps.

#### Examples:
1. Add 5 days to the current date:
   ```sql
   SELECT NOW() + INTERVAL '5 days';
   -- Output: Current date + 5 days
   ```

2. Subtract 2 hours from a timestamp:
   ```sql
   SELECT '2023-10-05 14:30:00'::TIMESTAMP - INTERVAL '2 hours';
   -- Output: 2023-10-05 12:30:00
   ```

3. Add 1 year and 6 months to a date:
   ```sql
   SELECT '2023-10-05'::DATE + INTERVAL '1 year 6 months';
   -- Output: 2025-04-05
   ```

---

### **3. Extracting Parts from an Interval**
You can use the `EXTRACT` function to extract specific parts (e.g., days, hours, minutes) from an interval.

#### Examples:
1. Extract the number of days from an interval:
   ```sql
   SELECT EXTRACT(DAY FROM INTERVAL '10 days 5 hours');
   -- Output: 10
   ```

2. Extract the number of hours from an interval:
   ```sql
   SELECT EXTRACT(HOUR FROM INTERVAL '10 days 5 hours');
   -- Output: 5
   ```

3. Extract the total number of seconds from an interval:
   ```sql
   SELECT EXTRACT(EPOCH FROM INTERVAL '2 hours 30 minutes');
   -- Output: 9000 (seconds)
   ```

---

### **4. Truncating Intervals**
PostgreSQL does not have a direct `DATE_TRUNC` equivalent for intervals, but you can manipulate intervals using arithmetic or functions.

#### Example:
If you want to truncate an interval to the nearest day:
```sql
SELECT INTERVAL '1 day' * EXTRACT(DAY FROM INTERVAL '3 days 5 hours');
-- Output: 3 days
```

---

### **5. Combining Intervals with `DATE_TRUNC`**
You can use `DATE_TRUNC` with intervals to perform operations like rounding timestamps and then adding/subtracting intervals.

#### Examples:
1. Truncate a timestamp to the start of the month and add 10 days:
   ```sql
   SELECT DATE_TRUNC('month', '2023-10-15 14:30:00'::TIMESTAMP) + INTERVAL '10 days';
   -- Output: 2023-10-11 00:00:00
   ```

2. Truncate a timestamp to the start of the hour and subtract 30 minutes:
   ```sql
   SELECT DATE_TRUNC('hour', '2023-10-05 14:30:00'::TIMESTAMP) - INTERVAL '30 minutes';
   -- Output: 2023-10-05 13:30:00
   ```

---

### **6. Interval in Table Queries**
You can use intervals in queries to filter or calculate date ranges.

#### Examples:
1. Find records created in the last 7 days:
   ```sql
   SELECT * FROM orders
   WHERE created_at >= NOW() - INTERVAL '7 days';
   ```

2. Calculate the difference between two timestamps as an interval:
   ```sql
   SELECT '2023-10-05 14:30:00'::TIMESTAMP - '2023-10-01 10:00:00'::TIMESTAMP;
   -- Output: 4 days 04:30:00
   ```

---

### **Summary of Key Points**
- **`INTERVAL`** represents a span of time.
- You can add/subtract intervals from dates or timestamps.
- Use `EXTRACT` to get specific parts (e.g., days, hours) from an interval.
- Combine `INTERVAL` with `DATE_TRUNC` for advanced date manipulations.

Let me know if you need further clarification or examples!