# 8. Aggregates and grouping

[Back to notes index](../README.md)

| [Previous: Joins and related tables](./07-joins-and-related-tables.md) | [Notes index](../README.md) | [Next: Subqueries, CTEs, and set operators](./09-subqueries-ctes-and-set-operators.md) |
| --- | --- | --- |

## Summarize a set of rows

Aggregate functions calculate a summary across rows. Common examples include **COUNT**, **SUM**, **AVG**, **MIN**, and **MAX**:

~~~sql
SELECT COUNT(*) AS employee_count,
       AVG(salary) AS average_salary,
       MIN(salary) AS lowest_salary,
       MAX(salary) AS highest_salary
FROM employees;
~~~

**COUNT(*)** counts rows. **COUNT(column)** counts rows where that column is not NULL. Most aggregate functions ignore NULL inputs. The result of **AVG(salary)** can therefore differ from an average that treats missing salary as zero.

## Group summaries by a column

Use **GROUP BY** to calculate one summary for each group:

~~~sql
SELECT department_id,
       COUNT(*) AS employee_count,
       ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY department_id
ORDER BY department_id NULLS LAST;
~~~

Every selected expression that is not aggregated must be included in the grouping or otherwise be valid for the selected database release and query form. Keep the selected values aligned with the grouping columns.

Rows with a NULL department value form a group of their own. Filter them before grouping if they should not appear.

## Filter groups with HAVING

**WHERE** filters input rows before groups are built. **HAVING** filters the completed groups:

~~~sql
SELECT department_id,
       COUNT(*) AS employee_count
FROM employees
WHERE salary >= 50000
GROUP BY department_id
HAVING COUNT(*) >= 2
ORDER BY employee_count DESC, department_id;
~~~

This query first removes employees below the salary threshold. It then forms groups and keeps departments with at least two remaining employees.

Use **WHERE** for row-level conditions and **HAVING** for aggregate conditions. Filtering rows earlier can reduce the work needed to build the groups.

## Count distinct values

**COUNT(DISTINCT expression)** counts the unique non-NULL values of an expression:

~~~sql
SELECT COUNT(DISTINCT department_id) AS assigned_departments
FROM employees;
~~~

The NULL department is not counted. If NULL itself needs to be represented, report it separately with a conditional count or a separate query instead of confusing it with a real identifier.

## Practice

Calculate the employee count and average salary by department. Add a WHERE condition for a salary range and a HAVING condition for a minimum group size. Compare **COUNT(*)** with **COUNT(commission_pct)** and explain why the values may differ.

## Check what you learned

1. What does an aggregate function calculate?
2. What is the difference between COUNT(*) and COUNT(column)?
3. How do most aggregate functions treat NULL inputs?
4. What does GROUP BY produce?
5. What happens to rows with a NULL grouping value?
6. When should WHERE be used?
7. When should HAVING be used?
8. What values does COUNT(DISTINCT column) count?

## References

- [Aggregate functions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Aggregate-Functions.html)
- [SELECT and GROUP BY](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)
- [COUNT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COUNT.html)