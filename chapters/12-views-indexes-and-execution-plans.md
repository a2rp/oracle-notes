# 12. Views, indexes, and execution plans

[Back to notes index](../README.md)

| [Previous: Keys, constraints, identity, and sequences](./11-keys-constraints-identity-and-sequences.md) | [Notes index](../README.md) | [Next: PL/SQL blocks, variables, and control flow](./13-plsql-blocks-variables-and-control-flow.md) |
| --- | --- | --- |

## Save a query as a view

A view gives a query a named interface:

~~~sql
CREATE OR REPLACE VIEW employee_directory AS
SELECT employee_id, full_name, department_id
FROM employees;
~~~

Query it like a table:

~~~sql
SELECT employee_id, full_name
FROM employee_directory
WHERE department_id = 20
ORDER BY employee_id;
~~~

A regular view stores its query definition, not a separate copy of the query rows. It can simplify access and expose only selected columns. A materialized view is a different object that stores results and needs a refresh strategy.

## Add an index for a query pattern

An index can help Oracle locate rows without scanning every table block, depending on the query, table size, and data distribution:

~~~sql
CREATE INDEX employees_department_salary_ix
ON employees (department_id, salary);
~~~

The order of columns in a composite index matters. An index beginning with **department_id** is shaped for a different set of lookup patterns from one beginning with **salary**.

Indexes use storage and add work to inserts, updates, and deletes. Do not create one for every column. Start with an observed query and validate that the index helps that workload.

## Ask Oracle for an execution plan

An execution plan describes the operations Oracle expects to use for a statement:

~~~sql
EXPLAIN PLAN FOR
SELECT employee_id, full_name
FROM employees
WHERE department_id = 20
ORDER BY salary;

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
~~~

The plan may show a table scan, index access, joins, sorting, and estimated row counts. A table scan is not automatically bad; it can be the right choice when many rows are needed or the table is small.

**EXPLAIN PLAN** shows an estimated plan for the statement. It may differ from the cursor plan chosen during execution because of binds, session settings, statistics, and runtime conditions. Use the plan tools available in the database and client, and make sure the session has the required privileges.

## Read plans with a question in mind

Look for the operations relevant to the query: where rows are filtered, whether an unexpected sort appears, and whether a join produces an unexpectedly large result. Compare estimates with observed rows when runtime plan information is available.

Do not tune from one plan line without checking the query result, table statistics, bind values, and representative data. Measure the workload before and after a change.

## Practice

Create the view and query it. Explain a filtered query, display its plan, and note whether Oracle chose a table or index access path. Add an index only if you can describe which query pattern it is intended to support.

## Check what you learned

1. What does a regular view store?
2. How does a materialized view differ from a regular view?
3. What can an index help Oracle do?
4. Why does a composite index's column order matter?
5. What costs do indexes add to data changes?
6. What does an execution plan describe?
7. Does a table scan always mean a query is poorly designed?
8. Why can an estimated plan differ from the plan used during execution?

## References

- [CREATE VIEW](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CREATE-VIEW.html)
- [CREATE INDEX](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CREATE-INDEX.html)
- [Oracle Database concepts: indexes and index-organized tables](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/indexes-and-index-organized-tables.html)
- [DBMS_XPLAN](https://docs.oracle.com/en/database/oracle/oracle-database/23/arpls/DBMS_XPLAN.html)