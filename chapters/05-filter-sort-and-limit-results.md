# 5. Filter, sort, and limit results

[Back to notes index](../README.md)

| [Previous: Select rows and shape results](./04-select-rows-and-shape-results.md) | [Notes index](../README.md) | [Next: SQL functions, NULL, and conversions](./06-sql-functions-null-and-conversions.md) |
| --- | --- | --- |

## Filter rows with WHERE

A **WHERE** condition keeps rows that satisfy a predicate:

~~~sql
SELECT employee_id, full_name, salary
FROM employees
WHERE salary >= 70000
  AND department_id IN (10, 20);
~~~

Use **AND** when all conditions must be true and **OR** when at least one condition may be true. Add parentheses when combining them so the intended logic is clear.

Other useful predicates include **IN** for membership, **BETWEEN** for an inclusive range, and **LIKE** for a text pattern:

~~~sql
SELECT employee_id, full_name, hire_date
FROM employees
WHERE hire_date BETWEEN DATE '2022-01-01' AND DATE '2024-12-31'
  AND full_name LIKE 'A%';
~~~

The percent sign matches zero or more characters. The underscore wildcard matches one character. If a pattern needs a literal wildcard, specify an escape character with **ESCAPE**.

## Handle NULL explicitly

**NULL** means the value is missing or unknown. It is not equal to zero, an empty string, or another NULL. Comparisons with NULL do not evaluate to true or false in the usual way.

Use **IS NULL** or **IS NOT NULL**:

~~~sql
SELECT employee_id, full_name
FROM employees
WHERE department_id IS NULL;
~~~

A predicate such as **department_id = NULL** does not find missing values. Remember that a row is returned only when the WHERE condition evaluates to true.

## Sort rows and place nulls

Use **ORDER BY** to make the result order explicit. Oracle allows null placement to be stated for each sort key:

~~~sql
SELECT employee_id, full_name, department_id
FROM employees
ORDER BY department_id ASC NULLS LAST, employee_id;
~~~

The employee identifier makes the ordering stable when department values tie. State **NULLS FIRST** or **NULLS LAST** when the display behavior matters rather than relying on a default.

## Limit the result size

Use the row-limiting clause to return a bounded result:

~~~sql
SELECT employee_id, full_name, salary
FROM employees
ORDER BY salary DESC, employee_id
FETCH FIRST 10 ROWS ONLY;
~~~

For a later page, skip an offset and fetch the next rows:

~~~sql
SELECT employee_id, full_name
FROM employees
ORDER BY employee_id
OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
~~~

Use a stable ordering when limiting rows. Otherwise, the database has no defined basis for deciding which rows belong in one page. Confirm row-limiting syntax against the Oracle Database release used by the application.

## Practice

Find employees hired within a date range whose name begins with a chosen letter. Include rows with no department separately using **IS NULL**. Sort the results and fetch a fixed number of rows with a deterministic tie-breaker.

## Check what you learned

1. What does a WHERE clause do?
2. When is IN useful?
3. Is BETWEEN inclusive of both endpoints?
4. What does NULL represent?
5. Why does equality comparison not find NULL values?
6. What does the percent wildcard match in a LIKE pattern?
7. Why should null placement be explicit when it matters?
8. Why does a row limit need a stable ORDER BY?

## References

- [SELECT and row limiting](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)
- [Conditions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Conditions.html)
- [Nulls](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Nulls.html)
- [Pattern-matching conditions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Pattern-matching-Conditions.html)