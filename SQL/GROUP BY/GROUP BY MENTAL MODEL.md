

#### **Key Concepts:**
1. **Purpose of `GROUP BY`**:
   - Groups data into **logical buckets** (e.g., by employee, department) to run **aggregate functions** (e.g., `MAX`, `COUNT`) per group.
   - Answers questions like:  
     - "What is the most recent salary bump date for each employee?"  
     - "What is the highest salary per department?"

2. **Mental Model**:
   - **Grouping**: Creates subsets of data where each group shares a common value (e.g., all records for employee ID 1001).
   - **Aggregation**: Applies functions to each group (e.g., `MAX(from_date)` for the latest salary change).

3. **Example Workflow**:
   ```sql
   SELECT 
       employee_number, 
       MAX(from_date) AS latest_raise_date,
       MAX(salary) AS highest_salary
   FROM 
       salaries
   GROUP BY 
       employee_number;
   ```
   - **Output**: One row per employee with their latest raise date and highest salary.

4. **Limitations**:
   - **Cannot Relate Aggregates Directly**:  
     - While you can get `MAX(from_date)` and `MAX(salary)` separately, you **cannot** guarantee they come from the same record in a single query.
     - Example: If an employee’s salary decreased (`90,000 → 70,000`), `MAX(salary)` won’t match their most recent salary.
   - **Over-Grouping Pitfall**:  
     Adding too many columns to `GROUP BY` (e.g., `salary`, `from_date`) splits data into overly granular groups, making aggregation meaningless.

5. **Key Insight**:
   - `GROUP BY` **reduces each group to one row** after aggregation.  
   - To correlate aggregated values (e.g., salary on the latest date), you’ll need **advanced techniques** (e.g., subqueries, window functions) covered later.

---

#### **Practical Example**:
**Goal**: Find each employee’s most recent salary bump date and the corresponding salary.  
**Challenge**: A naive approach fails if salaries fluctuate:
```sql
-- Problem: MAX(salary) might not match MAX(from_date)
SELECT 
    employee_number, 
    MAX(from_date) AS latest_date,
    MAX(salary) AS highest_salary  -- May not be the salary on latest_date!
FROM 
    salaries
GROUP BY 
    employee_number;
```

**Solution**: Requires multi-step queries (taught later).

---

#### **Why It Matters**:
- **Analytical Power**: Groups enable summaries like "average salary by department" or "employee count per hire year."
- **Limitations Awareness**: Understand that `GROUP BY` alone cannot always link aggregated values to specific records.
- **Foundation for Advanced SQL**: Prepares for learning subqueries, joins, and window functions to solve complex problems.

---

**Next Steps**:  
Practice grouping with real datasets and explore techniques to overcome limitations (e.g., using `JOIN` with subqueries).