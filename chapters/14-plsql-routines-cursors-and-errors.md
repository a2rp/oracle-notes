# 14. PL/SQL cursors, routines, packages, and errors

[Back to notes index](../README.md)

| [Previous: PL/SQL blocks, variables, and control flow](./13-plsql-blocks-variables-and-control-flow.md) | [Notes index](../README.md) | [Next: Users, privileges, backup, and recovery](./15-security-backup-and-recovery.md) |
| --- | --- | --- |

## Process query rows with a cursor FOR loop

A cursor represents a query result. A cursor FOR loop opens the query, fetches each row, and closes it automatically:

~~~sql
BEGIN
  FOR employee_row IN (
    SELECT employee_id, full_name
    FROM employees
    ORDER BY employee_id
  ) LOOP
    DBMS_OUTPUT.PUT_LINE(
      employee_row.employee_id || ': ' || employee_row.full_name
    );
  END LOOP;
END;
/
~~~

Prefer a set-based SQL statement when the work can be done for all rows together. Use procedural row-by-row logic only when the operation needs per-row procedural behavior.

## Inspect DML with implicit cursor attributes

After a DML statement, PL/SQL exposes information through the implicit cursor attributes. **SQL%ROWCOUNT** reports how many rows the most recent statement affected:

~~~sql
BEGIN
  UPDATE employees
  SET salary = salary * 1.02
  WHERE employee_id = 201;

  IF SQL%ROWCOUNT = 0 THEN
    RAISE_APPLICATION_ERROR(-20001, 'Employee was not found');
  END IF;
END;
/
~~~

This block deliberately does not commit. Let the calling transaction decide when the change should be committed or rolled back.

## Create a stored procedure

A procedure performs an operation and can accept parameters:

~~~sql
CREATE OR REPLACE PROCEDURE raise_employee_salary (
  p_employee_id IN employees.employee_id%TYPE,
  p_percent IN NUMBER
) AS
BEGIN
  IF p_percent IS NULL OR p_percent <= 0 OR p_percent > 25 THEN
    RAISE_APPLICATION_ERROR(-20002, 'Raise percent is outside the allowed range');
  END IF;

  UPDATE employees
  SET salary = salary * (1 + p_percent / 100)
  WHERE employee_id = p_employee_id;

  IF SQL%ROWCOUNT = 0 THEN
    RAISE_APPLICATION_ERROR(-20001, 'Employee was not found');
  END IF;
END;
/
~~~

The procedure raises a named application error for invalid input or a missing employee. The client can handle the failure and control the transaction. Execute the routine with a PL/SQL block or the client syntax supported by the connected tool.

## Create a function that returns a value

A function declares a return type and returns a value on its successful path:

~~~sql
CREATE OR REPLACE FUNCTION annual_salary (
  p_employee_id IN employees.employee_id%TYPE
) RETURN NUMBER AS
  v_salary employees.salary%TYPE;
BEGIN
  SELECT salary
  INTO v_salary
  FROM employees
  WHERE employee_id = p_employee_id;

  RETURN v_salary * 12;
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    RAISE_APPLICATION_ERROR(-20001, 'Employee was not found');
END;
/
~~~

A function called from SQL has restrictions on what it may do. Keep SQL-callable functions focused on returning values and check the SQL and PL/SQL rules for side effects.

## Group related routines in a package

A package specification declares public procedures, functions, types, and values:

~~~sql
CREATE OR REPLACE PACKAGE employee_api AS
  PROCEDURE raise_salary (
    p_employee_id IN employees.employee_id%TYPE,
    p_percent IN NUMBER
  );
END employee_api;
/
~~~

The package body supplies the implementation:

~~~sql
CREATE OR REPLACE PACKAGE BODY employee_api AS
  PROCEDURE raise_salary (
    p_employee_id IN employees.employee_id%TYPE,
    p_percent IN NUMBER
  ) AS
  BEGIN
    raise_employee_salary(p_employee_id, p_percent);
  END raise_salary;
END employee_api;
/
~~~

The specification is the public interface. The body may contain private helper routines that callers cannot invoke directly. Compile the called procedure before compiling this body, or place the implementation directly in the package body.

## Practice

Write a cursor FOR loop that prints employee names. Create a procedure that updates one employee and reports a missing row with an application error. Group a public routine in a package and inspect compilation errors with the SQL client when you intentionally misspell a column.

## Check what you learned

1. What work does a cursor FOR loop handle automatically?
2. When is set-based SQL preferable to row-by-row processing?
3. What does SQL%ROWCOUNT report?
4. Why might a procedure raise an application error for a missing row?
5. What distinguishes a procedure from a function?
6. What does a function declare and return?
7. What does a package specification contain?
8. Which package contents can remain private in the body?

## References

- [PL/SQL cursors](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/cursors-overview.html)
- [PL/SQL subprograms](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/plsql-subprograms.html)
- [PL/SQL packages](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/plsql-packages.html)
- [RAISE_APPLICATION_ERROR](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/raising-exceptions-explicitly.html)