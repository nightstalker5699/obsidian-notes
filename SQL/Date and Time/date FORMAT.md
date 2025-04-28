In PostgreSQL, you can format dates using the `TO_CHAR` function, which allows you to convert a date, timestamp, or interval to a string with a specified format. Here's how you can use it:

### Syntax:
```sql
TO_CHAR(date_expression, format_string)
```

### Common Format Patterns:
- **YYYY**: 4-digit year (e.g., 2023)
- **YY**: 2-digit year (e.g., 23)
- **MM**: Month as a 2-digit number (e.g., 01 for January)
- **Mon**: Abbreviated month name (e.g., Jan)
- **Month**: Full month name (e.g., January)
- **DD**: Day of the month as a 2-digit number (e.g., 05)
- **Day**: Full day name (e.g., Monday)
- **Dy**: Abbreviated day name (e.g., Mon)
- **HH24**: Hour of the day (00 to 23)
- **HH12**: Hour of the day (01 to 12)
- **MI**: Minute (00 to 59)
- **SS**: Second (00 to 59)
- **AM/PM**: Meridian indicator (e.g., AM or PM)

### Examples:
1. Format a date as `YYYY-MM-DD`:
   ```sql
   SELECT TO_CHAR(NOW(), 'YYYY-MM-DD');
   -- Output: 2023-10-05
   ```

2. Format a date as `Month DD, YYYY`:
   ```sql
   SELECT TO_CHAR(NOW(), 'Month DD, YYYY');
   -- Output: October 05, 2023
   ```

3. Format a timestamp as `Day, DD Mon YYYY HH24:MI:SS`:
   ```sql
   SELECT TO_CHAR(NOW(), 'Day, DD Mon YYYY HH24:MI:SS');
   -- Output: Thursday, 05 Oct 2023 14:35:12
   ```

4. Format a date as `MM/DD/YY`:
   ```sql
   SELECT TO_CHAR(NOW(), 'MM/DD/YY');
   -- Output: 10/05/23
   ```

5. Format a date with a literal string:
   ```sql
   SELECT TO_CHAR(NOW(), '"Today is" Day, Month DD, YYYY');
   -- Output: Today is Thursday, October 05, 2023
   ```

### Notes:
- The `NOW()` function returns the current date and time.
- You can use double quotes (`"`) to include literal text in the format string.
- For more formatting options, refer to the [PostgreSQL documentation](https://www.postgresql.org/docs/current/functions-formatting.html).

Let me know if you need further clarification!