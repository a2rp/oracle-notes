# 3. Create schemas, tables, and Oracle data types

[Back to notes index](../README.md)

| [Previous: Connect with SQL Developer and SQLcl](./02-connect-with-sql-developer-and-sqlcl.md) | [Notes index](../README.md) | [Next: Select rows and shape results](./04-select-rows-and-shape-results.md) |
| --- | --- | --- |

## Work inside a schema

A schema owns objects such as tables, views, indexes, sequences, and PL/SQL routines. After connecting as a practice user, a table created without a schema prefix belongs to that user's schema.

Oracle stores ordinary unquoted identifiers in uppercase in the data dictionary. Quoted identifiers preserve their written case and must be quoted consistently, so use simple unquoted names for ordinary tables and columns.

## Choose data types for the values

Common Oracle data types include:

- **VARCHAR2** for variable-length text
- **CHAR** for fixed-length text
- **NUMBER(p, s)** for numeric values with optional precision and scale
- **DATE** for a date and time down to seconds
- **TIMESTAMP** for fractional seconds, with variants that can include time-zone information
- **CLOB** for large character data
- **BLOB** for large binary data

A **DATE** column stores time as well as a calendar date. Do not assume that it stores only a date. Choose **TIMESTAMP** when fractional seconds or time-zone behavior is part of the requirement.

## Create a practice table

Run this statement while connected to a disposable practice schema:

~~~sql
CREATE TABLE employees (
  employee_id NUMBER,
  full_name VARCHAR2(100 CHAR),
  salary NUMBER(10, 2),
  hire_date DATE,
  biography CLOB,
  profile_photo BLOB
);
~~~

The **CHAR** length qualifier says the text limit is expressed in characters. Oracle also supports byte-based semantics. Choose a clear convention for multilingual text and document it for the schema.

The table is intentionally simple here. The next chapter on constraints adds a primary key and validation rules.

## Inspect columns

A SQL*Plus-compatible client can describe a table with:

~~~sql
DESC employees
~~~

**DESC** is a client command. Query the data dictionary when metadata needs to be used in SQL:

~~~sql
SELECT column_name, data_type, data_length, data_precision, data_scale
FROM user_tab_columns
WHERE table_name = 'EMPLOYEES'
ORDER BY column_id;
~~~

Because the table was created with an unquoted name, the dictionary stores its name in uppercase. **USER_TAB_COLUMNS** describes columns of objects owned by the current user.

## Insert values that match the types

Use explicit date formats or Oracle date literals when writing dates in a script:

~~~sql
INSERT INTO employees (
  employee_id, full_name, salary, hire_date
)
VALUES (
  101, 'Asha Rao', 85000.00, DATE '2024-01-15'
);
~~~

The date literal uses the format **DATE 'YYYY-MM-DD'** and does not depend on a session display format. The insert starts a transaction; it is not durable until committed.

## Practice

Create the table, inspect its columns, insert a row with a date literal, and query the row. Add a timestamp column to a second practice table and explain why **DATE** and **TIMESTAMP** may represent different precision requirements.

## Check what you learned

1. What kinds of objects belong to a schema?
2. Which schema owns a table created without a schema prefix?
3. How does Oracle store ordinary unquoted identifiers?
4. When is VARCHAR2 suitable?
5. What is the difference between NUMBER precision and scale?
6. Does the Oracle DATE type store a time component?
7. Which types can hold large character and binary values?
8. Why is an Oracle date literal useful in a script?

## References

- [Oracle Database SQL data types](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Data-Types.html)
- [CREATE TABLE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/CREATE-TABLE.html)
- [USER_TAB_COLUMNS](https://docs.oracle.com/en/database/oracle/oracle-database/23/refrn/USER_TAB_COLUMNS.html)
- [Oracle Database concepts: schemas and objects](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/schema-objects.html)