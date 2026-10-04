# 13. PL/SQL blocks, variables, and control flow

[Back to notes index](../README.md)

| [Previous: Views, indexes, and execution plans](./12-views-indexes-and-execution-plans.md) | [Notes index](../README.md) | [Next: PL/SQL cursors, routines, packages, and errors](./14-plsql-routines-cursors-and-errors.md) |
| --- | --- | --- |

## Structure an anonymous block

PL/SQL combines SQL with procedural statements. An anonymous block has an optional declaration section, a required executable section, and an optional exception section:

~~~sql
DECLARE
  v_message VARCHAR2(80) := 'Hello from PL/SQL';
BEGIN
  DBMS_OUTPUT.PUT_LINE(v_message);
END;
/
~~~

Enable DBMS Output in the client to see the printed message. The semicolons belong to PL/SQL statements. The slash on a separate line is a SQL*Plus-style client command that submits the completed block.

## Declare values using table definitions

Use **%TYPE** to give a variable the type of a table column:

~~~sql
DECLARE
  v_employee_id employees.employee_id%TYPE := 201;
  v_name employees.full_name%TYPE;
BEGIN
  SELECT full_name
  INTO v_name
  FROM employees
  WHERE employee_id = v_employee_id;

  DBMS_OUTPUT.PUT_LINE(v_name);
END;
/
~~~

If the query returns no row, PL/SQL raises **NO_DATA_FOUND**. If it returns more than one row, it raises **TOO_MANY_ROWS**. Use **SELECT INTO** only when the result is expected to contain exactly one row.

**%ROWTYPE** declares a record with fields matching a table row or cursor result:

~~~sql
DECLARE
  v_employee employees%ROWTYPE;
BEGIN
  SELECT *
  INTO v_employee
  FROM employees
  WHERE employee_id = 201;

  DBMS_OUTPUT.PUT_LINE(v_employee.full_name);
END;
/
~~~

A record is useful when several related fields are read together. Selecting explicit columns is usually clearer when the block only needs a few values.

## Use conditions and loops

PL/SQL supports IF conditions and loop forms:

~~~sql
DECLARE
  v_salary employees.salary%TYPE := 75000;
BEGIN
  IF v_salary >= 80000 THEN
    DBMS_OUTPUT.PUT_LINE('Higher salary range');
  ELSIF v_salary >= 50000 THEN
    DBMS_OUTPUT.PUT_LINE('Middle salary range');
  ELSE
    DBMS_OUTPUT.PUT_LINE('Lower salary range');
  END IF;

  FOR item_number IN 1..3 LOOP
    DBMS_OUTPUT.PUT_LINE('Item ' || item_number);
  END LOOP;
END;
/
~~~

The loop variable in a numeric **FOR** loop is managed by PL/SQL. Use a loop when the number of iterations or the result of each step is meaningful. Avoid unnecessary procedural loops when a single SQL statement can handle the rows set-wise.

## Handle expected exceptions

An exception section handles errors raised while the block runs:

~~~sql
DECLARE
  v_name employees.full_name%TYPE;
BEGIN
  SELECT full_name
  INTO v_name
  FROM employees
  WHERE employee_id = -1;

  DBMS_OUTPUT.PUT_LINE(v_name);
EXCEPTION
  WHEN NO_DATA_FOUND THEN
    DBMS_OUTPUT.PUT_LINE('No employee matched');
END;
/
~~~

Handle expected cases deliberately. For unexpected failures, allow the error to reach an appropriate caller or log useful context rather than returning a success-shaped result.

## Practice

Run the anonymous block and enable DBMS Output. Use **%TYPE** for an employee identifier and salary, then read one row with **SELECT INTO**. Try a missing identifier and observe the exception. Add a condition and a small numeric loop.

## Check what you learned

1. Which sections can appear in an anonymous PL/SQL block?
2. Which section must be present?
3. What does %TYPE copy from a table column?
4. What does %ROWTYPE describe?
5. What happens when SELECT INTO returns no rows?
6. What happens when SELECT INTO returns multiple rows?
7. What does the slash do after a block in a SQL*Plus-style client?
8. Why prefer set-based SQL over an unnecessary procedural loop?

## References

- [PL/SQL language fundamentals](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/plsql-language-fundamentals.html)
- [PL/SQL static SQL](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/static-sql.html)
- [PL/SQL error handling](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/plsql-error-handling.html)