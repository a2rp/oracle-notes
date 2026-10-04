# 7. Joins and related tables

[Back to notes index](../README.md)

| [Previous: SQL functions, NULL, and conversions](./06-sql-functions-null-and-conversions.md) | [Notes index](../README.md) | [Next: Aggregates and grouping](./08-aggregates-and-grouping.md) |
| --- | --- | --- |

## Prepare two related tables

A join combines rows from related tables. This example uses the department identifier stored on an employee and on its department:

~~~sql
CREATE TABLE departments (
  department_id NUMBER,
  department_name VARCHAR2(80 CHAR)
);

INSERT INTO departments (department_id, department_name)
VALUES (10, 'Sales');

INSERT INTO departments (department_id, department_name)
VALUES (20, 'Engineering');

INSERT INTO employees (employee_id, full_name, department_id)
VALUES (201, 'Asha Rao', 10);

INSERT INTO employees (employee_id, full_name, department_id)
VALUES (202, 'Ravi Shah', NULL);
~~~

The example tables are simple practice objects. A later chapter adds keys and referential constraints.

## Return only matching rows with INNER JOIN

An **INNER JOIN** returns rows where the join condition matches:

~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
INNER JOIN departments d
  ON d.department_id = e.department_id
ORDER BY e.employee_id;
~~~

The aliases **e** and **d** shorten references and show which table owns each column. Qualify columns when names repeat across tables.

Employee 201 matches a department. Employee 202 has no department identifier, so it does not appear in this inner join.

## Keep all rows from one side with LEFT JOIN

A **LEFT JOIN** returns every left-side row and the matching right-side values when they exist. Missing matches produce NULL columns from the right side:

~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
  ON d.department_id = e.department_id
ORDER BY e.employee_id;
~~~

Employee 202 remains in the output, with a NULL department name. Use this shape when the left-side record should remain visible even if the related row is missing.

## Put optional-side filters in the right place

This query keeps all employees and matches only the Engineering department:

~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
  ON d.department_id = e.department_id
 AND d.department_name = 'Engineering'
ORDER BY e.employee_id;
~~~

Moving **d.department_name = 'Engineering'** into the WHERE clause would filter out rows where the right side is NULL, changing the result to behave like an inner filter. Decide whether the condition limits matching departments or filters the final result.

## Understand row multiplication

If one employee can have several matching child rows, a join returns one employee row for each matching child row. This is expected for a one-to-many relationship. If a result has more rows than expected, check the relationship and join condition before adding **DISTINCT**.

A self-join uses the same table twice with different aliases, such as matching employees to their managers. Use clear aliases so the two roles stay distinct.

## Practice

Run the setup statements once in a disposable schema. Compare the inner and left joins. Move the department-name condition between **ON** and **WHERE**, then explain which employee rows disappear and why.

## Check what you learned

1. What does an INNER JOIN return?
2. What does a LEFT JOIN preserve?
3. Which side's columns become NULL when a left-side row has no match?
4. Why qualify repeated column names with table aliases?
5. How can a predicate in WHERE change a left join's result?
6. Why can one parent row appear multiple times after a join?
7. Why should duplicate results be investigated before adding DISTINCT?
8. What is a self-join?

## References

- [Joins](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Joins.html)
- [SELECT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/SELECT.html)
- [Oracle Database concepts: data integrity](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/data-integrity.html)