### **Summary of "109 - HAVING Keyword English.txt"**

#### **Key Concepts:**
1. **Purpose of `HAVING`**:
   - Filters **groups** created by `GROUP BY` (unlike `WHERE`, which filters individual rows).
   - Used to apply conditions on **aggregated results** (e.g., "show departments with >25,000 employees").

2. **Syntax & Placement**:
   ```sql
   SELECT department_name, COUNT(employee_number) AS employee_count
   FROM employees
   JOIN department_employee ON employees.id = department_employee.employee_id
   JOIN departments ON department_employee.department_id = departments.id
   GROUP BY department_name
   HAVING COUNT(employee_number) > 25000;
   ```
   - **Order of Operations**: `FROM` → `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`.

3. **`HAVING` vs. `WHERE`**:
   - **`WHERE`**: Filters **rows before grouping** (e.g., `WHERE gender = 'F'`).
   - **`HAVING`**: Filters **groups after aggregation** (e.g., `HAVING COUNT(employee_number) > 25000`).
   - **Critical Rule**: Aggregate functions (e.g., `COUNT`, `SUM`) **cannot** be used in `WHERE` but **can** in `HAVING`.

4. **Example Workflow**:
   - **Step 1**: Join tables to link employees to departments.
   - **Step 2**: Group by `department_name` to count employees per department.
   - **Step 3**: Use `HAVING` to filter departments with >25,000 employees.
   - **Optional**: Add `WHERE` to pre-filter rows (e.g., only female employees).

5. **Common Use Cases**:
   - Find departments with high turnover (`HAVING COUNT(resignations) > 100`).
   - Identify high-revenue products (`HAVING SUM(sales) > 1M`).
   - Filter groups based on aggregated metrics (averages, totals, etc.).

---

#### **Key Differences Illustrated**:
| **Clause** | **Applies To**          | **Aggregate Functions?** | **Example**                          |
|------------|-------------------------|--------------------------|--------------------------------------|
| `WHERE`    | Individual rows         | ❌ No                    | `WHERE salary > 50000`               |
| `HAVING`   | Groups (after `GROUP BY`)| ✅ Yes                   | `HAVING AVG(salary) > 60000`         |

---

#### **Practical Example**:
**Goal**: Find departments with >25,000 **female** employees.  
**Query**:
```sql
SELECT 
    d.department_name, 
    COUNT(e.employee_number) AS female_employees
FROM 
    employees e
JOIN 
    department_employee de ON e.id = de.employee_id
JOIN 
    departments d ON de.department_id = d.id
WHERE 
    e.gender = 'F'  -- Filter rows FIRST (individual female employees)
GROUP BY 
    d.department_name
HAVING 
    COUNT(e.employee_number) > 25000;  -- Filter groups AFTER aggregation
```
**Output**:
| department_name | female_employees |
|-----------------|-------------------|
| Development     | 34,258            |
| Production      | 29,549            |

---

#### **Why It Matters**:
- **Precision**: Combines `WHERE` (row-level filtering) and `HAVING` (group-level filtering) for granular analysis.
- **Performance**: Filters data early with `WHERE`, reducing the load for aggregation.
- **Business Insights**: Answers questions like "Which departments meet specific performance thresholds?"

---

**Next Steps**: Practice with exercises to master `HAVING` in complex queries (e.g., nested aggregates, multi-table joins).