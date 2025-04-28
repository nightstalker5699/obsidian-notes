In PostgreSQL, you can calculate age using the `AGE` function. This function returns the interval between two dates (or the current date and a specified date) in years, months, and days. It is commonly used to calculate a person's age.

### Syntax:
```sql
AGE(timestamp) -- Calculates age from the current date
AGE(timestamp, timestamp) -- Calculates age between two specified dates
```

### Examples:

1. **Calculate age from a birthdate to the current date:**
   ```sql
   SELECT AGE('1990-05-15');
   -- Output: 33 years 4 months 20 days (assuming today is 2023-10-05)
   ```

2. **Calculate age between two specific dates:**
   ```sql
   SELECT AGE('2000-01-01', '1990-05-15');
   -- Output: 9 years 7 months 16 days
   ```

3. **Extract only the years from the age:**
   If you want just the number of years (e.g., for a person's age), you can use the `DATE_PART` function:
   ```sql
   SELECT DATE_PART('year', AGE('1990-05-15'));
   -- Output: 33
   ```

4. **Calculate age for a table column:**
   If you have a table with a `birthdate` column, you can calculate the age for each row:
   ```sql
   SELECT name, AGE(birthdate) AS age FROM users;
   -- Output: Name | Age
   --         John | 33 years 4 months 20 days
   ```

5. **Format the age output:**
   If you want to format the age in a specific way (e.g., only years and months), you can use `TO_CHAR` with `AGE`:
   ```sql
   SELECT TO_CHAR(AGE('1990-05-15'), 'YY "years" MM "months"');
   -- Output: 33 years 04 months
   ```

### Notes:
- The `AGE` function returns an interval, which includes years, months, and days.
- If you only need the number of years, use `DATE_PART('year', AGE(...))`.
- The `AGE` function is very useful for calculating ages in applications like HR systems, user profiles, etc.

Let me know if you need further assistance!