# SQL Conditional Functions with Examples

Here's a practical guide to using NULLIF and CASE statements in SQL, with clear examples from the transcripts:

## 1. NULLIF Function

**Purpose**: Returns NULL if two values are equal, otherwise returns the first value.

### Basic Syntax:
```sql
NULLIF(value1, value2)
```

### Example 1: Basic Usage
```sql
SELECT 
    NULLIF(0, 0) AS example1,      -- Returns NULL
    NULLIF('ABC', 'DEF') AS example2; -- Returns 'ABC'
```

### Example 2: Preventing Division by Zero
```sql
SELECT 
    numerator / NULLIF(denominator, 0) AS safe_division
FROM calculations;
```
*Returns NULL instead of error when denominator is 0*

### Example 3: Cleaning Data
```sql
SELECT 
    product_name,
    NULLIF(discount_price, regular_price) AS special_discount
FROM products;
```
*Shows NULL when discount equals regular price (no actual discount)*

## 2. CASE Statements

**Purpose**: Conditional logic in SQL queries that can be used in SELECT, WHERE, and other clauses.

### Basic Syntax:
```sql
CASE 
    WHEN condition1 THEN result1
    WHEN condition2 THEN result2
    ...
    ELSE default_result
END
```

### Example 1: Value Transformation
```sql
SELECT 
    customer_name,
    CASE 
        WHEN customer_id = 1 THEN 'First Customer'
        ELSE 'Regular Customer'
    END AS customer_type
FROM customers;
```

### Example 2: Conditional Filtering in WHERE
```sql
SELECT *
FROM orders
WHERE 
    CASE 
        WHEN customer_id > 10 THEN net_amount < 100
        ELSE net_amount > 100
    END;
```
*Filters differently based on customer_id*

### Example 3: Conditional Aggregation
```sql
SELECT 
    SUM(
        CASE 
            WHEN net_amount < 100 THEN net_amount - 100  -- Refund $100
            ELSE net_amount
        END
    ) AS adjusted_sales
FROM orders;
```
*Calculates potential refund impact*

### Example 4: Multiple Conditions
```sql
SELECT 
    product_name,
    price,
    CASE
        WHEN price < 50 THEN 'Budget'
        WHEN price BETWEEN 50 AND 200 THEN 'Mid-range'
        WHEN price > 200 THEN 'Premium'
        ELSE 'Unpriced'
    END AS price_category
FROM products;
```

## Key Differences:

| Feature        | NULLIF                          | CASE                           |
|----------------|---------------------------------|--------------------------------|
| Purpose        | Compare exactly 2 values        | Handle multiple conditions     |
| Best for       | Simple equality checks          | Complex conditional logic      |
| Returns        | NULL or first argument          | Any specified result           |
| Use cases      | Data cleaning, divide-by-zero   | Value transformation, filtering|

## Practical Tips:
1. Use NULLIF for simple "if equal then null" scenarios
2. Use CASE for more complex conditional logic
3. CASE can be used in SELECT, WHERE, GROUP BY, and ORDER BY clauses
4. NULLIF is often used with COALESCE to provide default values for NULL results

These conditional functions provide powerful ways to handle business logic directly in your SQL queries.