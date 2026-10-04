# 9. Subqueries, CTEs, and set operators

[Back to notes index](../README.md)

| [Previous: Aggregates and grouping](./08-aggregates-and-grouping.md) | [Notes index](../README.md) | [Next: Insert, update, delete, and transactions](./10-data-changes-and-transactions.md) |
| --- | --- | --- |

## Use a subquery for a related value

A subquery is a query nested inside another SQL statement. A scalar subquery returns one value and can be compared with a column:

~~~sql
SELECT employee_id, full_name, salary
FROM employees
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
)
ORDER BY salary DESC, employee_id;
~~~

The inner query calculates the average. The outer query returns employees whose salary exceeds that value.

## Use EXISTS for matching rows

**EXISTS** tests whether a subquery returns at least one row:

~~~sql
SELECT e.employee_id, e.full_name
FROM employees e
WHERE EXISTS (
  SELECT 1
  FROM departments d
  WHERE d.department_id = e.department_id
    AND d.department_name = 'Engineering'
);
~~~

This correlated subquery refers to **e.department_id** from the outer query. **EXISTS** expresses that a related row must be present without needing to return columns from the inner query.

Use **NOT EXISTS** when the requirement is to find rows with no matching related record. Be careful with NULL behavior when choosing between **NOT IN** and **NOT EXISTS**.

## Name a query with a common table expression

A **WITH** clause gives a subquery a name for one statement:

~~~sql
WITH department_pay AS (
  SELECT department_id,
         COUNT(*) AS employee_count,
         AVG(salary) AS average_salary
  FROM employees
  GROUP BY department_id
)
SELECT department_id,
       employee_count,
       ROUND(average_salary, 2) AS average_salary
FROM department_pay
WHERE average_salary > (
  SELECT AVG(salary)
  FROM employees
)
ORDER BY department_id NULLS LAST;
~~~

The CTE separates the grouped calculation from the final filter and projection. A CTE is part of the statement; it does not automatically create a stored table.

## Combine compatible result sets

Set operators combine the results of two queries. Each branch must return a compatible number of columns with compatible data types:

~~~sql
SELECT employee_id
FROM employees
WHERE salary >= 90000
UNION
SELECT employee_id
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY employee_id;
~~~

**UNION** removes duplicate rows. **UNION ALL** keeps them and is often more efficient when duplicates are meaningful or known not to occur. **INTERSECT** returns rows present in both sets. Oracle uses **MINUS** to return rows from the first set that are absent from the second.

Place **ORDER BY** once at the end to sort the combined result.

## Practice

Find employees earning more than the company average. Rewrite a related-row check using **EXISTS**. Build a CTE for department counts, then compare the identifiers returned by **UNION** and **UNION ALL**.

## Check what you learned

1. What does a scalar subquery return?
2. What does EXISTS test?
3. Why is a subquery correlated when it refers to an outer query alias?
4. What is a CTE?
5. Does a CTE create a permanent table?
6. What must be compatible between the branches of a set operator?
7. How does UNION differ from UNION ALL?
8. Which Oracle set operator subtracts the second result from the first?

## References

- [Subqueries](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Using-Subqueries.html)
- [SELECT and subquery factoring](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)
- [Set operators](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/The-UNION-ALL-INTERSECT-MINUS-Operators.html)