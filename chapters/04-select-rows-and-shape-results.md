# 4. Select rows and shape results

[Back to notes index](../README.md)

| [Previous: Create schemas, tables, and Oracle data types](./03-schemas-tables-and-oracle-data-types.md) | [Notes index](../README.md) | [Next: Filter, sort, and limit results](./05-filter-sort-and-limit-results.md) |
| --- | --- | --- |

## Choose the columns to return

A **SELECT** statement reads data. List the columns needed by the caller instead of selecting every column:

~~~sql
SELECT employee_id, full_name, salary
FROM employees;
~~~

Selecting only needed columns makes the result easier to understand and can reduce unnecessary data transfer.

## Return calculated expressions

A query can calculate a value without changing the stored row:

~~~sql
SELECT employee_id,
       full_name,
       salary,
       salary * 12 AS annual_salary
FROM employees;
~~~

The alias **ANNUAL_SALARY** labels the calculated column in the result. Use an alias that describes the value clearly. An alias changes the result heading, not the stored table definition.

## Use distinct values when needed

**DISTINCT** removes duplicate rows from the selected result:

~~~sql
SELECT DISTINCT department_id
FROM employees
ORDER BY department_id;
~~~

When more than one column is selected, Oracle removes duplicate combinations of all selected columns. Do not add **DISTINCT** just to hide unexpected duplicates. Check whether a join or data relationship is producing more rows than intended.

## Read a one-row expression

Oracle includes the **DUAL** table for evaluating expressions that do not need application table rows:

~~~sql
SELECT 2 + 3 AS result
FROM dual;
~~~

It is also useful for checking session values or simple built-in functions:

~~~sql
SELECT USER AS connected_user,
       SYSDATE AS database_time
FROM dual;
~~~

## Order results deliberately

Rows have no guaranteed display order unless the query includes **ORDER BY**. For a repeatable result, include a column or set of columns that uniquely orders rows:

~~~sql
SELECT employee_id, full_name, salary
FROM employees
ORDER BY salary DESC, employee_id;
~~~

The first key sorts salary from highest to lowest. The employee identifier breaks ties. Without an explicit unique tie-breaker, equally ranked rows can appear in a different order.

## Practice

Select an employee's identifier, name, and monthly salary. Add a calculated annual salary, then sort by it and use the employee identifier as a tie-breaker. Select distinct department identifiers and explain what a duplicate row means in that result.

## Check what you learned

1. What does SELECT do?
2. Why should a query list only the columns its caller needs?
3. What does a column alias change?
4. What does DISTINCT remove?
5. Why should DISTINCT not be used to hide join mistakes?
6. What is the DUAL table useful for?
7. Does a table guarantee row order without ORDER BY?
8. Why add a unique tie-breaker to ORDER BY?

## References

- [SELECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)
- [Expressions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Expressions.html)
- [DUAL table](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Selecting-from-the-DUAL-Table.html)