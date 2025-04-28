
#### **Key Concepts:**
1. **Purpose of `GROUPING SETS`**:
   - Combines results from **multiple `GROUP BY` queries** into a single output.
   - Avoids writing separate `UNION` queries for each grouping level.

2. **Syntax & Workflow**:
   ```sql
   SELECT 
       COALESCE(product_id, 'Total') AS category,
       SUM(quantity) AS total_quantity
   FROM 
       order_lines
   GROUP BY 
       GROUPING SETS (
           (),              -- Grand total (no grouping)
           (product_id)     -- Group by product_id
       )
   ORDER BY 
       category;
   ```
   - **Output**: One table showing both the **grand total** (all products) and **per-product totals**.

3. **Comparison to `UNION`**:
   - **`UNION`**: Merges results of separate queries (e.g., one for grand total, one for product totals).
   - **`GROUPING SETS`**: Does the same in a single query, improving readability and performance.

4. **Use Cases**:
   - **Hierarchical Summaries**:  
     - Show totals at different levels (e.g., by product, by region, and overall).
   - **Multi-Dimensional Analysis**:  
     - Combine groupings like `(product_id, year)` and `(region, year)` in one query.

5. **Example**:
   ```sql
   -- Using GROUPING SETS to show sales by product + grand total
   SELECT 
       product_id,
       SUM(quantity) AS total_sold
   FROM 
       order_lines
   GROUP BY 
       GROUPING SETS (
           (product_id), 
           ()
       )
   ORDER BY 
       product_id NULLS FIRST;  -- Grand total appears first (NULL product_id)
   ```
   **Output**:
   | product_id | total_sold |
   |------------|------------|
   | NULL       | 120,790    | ← Grand total
   | 1          | 9          |
   | 2          | 15         |

6. **Advanced Features**:
   - **`ROLLUP`**: Generates hierarchical subtotals (e.g., `(product, year)` → `(product)` → `()`).
   - **`CUBE`**: Generates all possible grouping combinations (e.g., `(product, year)`, `(product)`, `(year)`, `()`).

---

#### **Why It Matters**:
- **Efficiency**: Reduces query complexity by consolidating multiple aggregations.
- **Flexibility**: Answers multi-level business questions like:
  - "What are the sales per product **and** the overall total?"
  - "What are the sales by region **and** by year?"
- **Performance**: Optimized for large datasets vs. multiple `UNION` queries.

---

**Next Steps**:  
Practice with `GROUPING SETS`, `ROLLUP`, and `CUBE` to master multi-dimensional aggregation.