# All code samples

[Back to notes index](../README.md)

| [Previous: Use Oracle Database from JavaScript](./16-oracle-database-with-javascript.md) | [Notes index](../README.md) | [Next: Complete questions and answers](./99-complete-q-and-a.md) |
| --- | --- | --- |

This appendix gathers the examples from the core chapters in chapter order. Return to each chapter for the explanation, setup details, and practice notes around a sample.

## 1. Oracle Database and relational foundations

### Sample 1 (sql)

~~~~sql
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') AS container_name
FROM dual;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
~~~~

## 2. Connect with SQL Developer and SQLcl

### Sample 1 (sql)

~~~~sql
SELECT USER AS connected_user,
       SYS_CONTEXT('USERENV', 'CON_NAME') AS container_name
FROM dual;
~~~~

### Sample 2 (sql)

~~~~sql
SHOW USER
~~~~

### Sample 3 (sql)

~~~~sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
~~~~

### Sample 4 (sql)

~~~~sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('Connected to Oracle');
END;
/
~~~~

## 3. Create schemas, tables, and Oracle data types

### Sample 1 (sql)

~~~~sql
CREATE TABLE employees (
  employee_id NUMBER,
  full_name VARCHAR2(100 CHAR),
  department_id NUMBER,
  salary NUMBER(10, 2),
  commission_pct NUMBER(4, 3),
  hire_date DATE,
  biography CLOB,
  profile_photo BLOB
);
~~~~

### Sample 2 (sql)

~~~~sql
DESC employees
~~~~

### Sample 3 (sql)

~~~~sql
SELECT column_name, data_type, data_length, data_precision, data_scale
FROM user_tab_columns
WHERE table_name = 'EMPLOYEES'
ORDER BY column_id;
~~~~

### Sample 4 (sql)

~~~~sql
INSERT INTO employees (
  employee_id, full_name, salary, hire_date
)
VALUES (
  101, 'Asha Rao', 85000.00, DATE '2024-01-15'
);
~~~~

## 4. Select rows and shape results

### Sample 1 (sql)

~~~~sql
SELECT employee_id, full_name, salary
FROM employees;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT employee_id,
       full_name,
       salary,
       salary * 12 AS annual_salary
FROM employees;
~~~~

### Sample 3 (sql)

~~~~sql
SELECT DISTINCT department_id
FROM employees
ORDER BY department_id;
~~~~

### Sample 4 (sql)

~~~~sql
SELECT 2 + 3 AS result
FROM dual;
~~~~

### Sample 5 (sql)

~~~~sql
SELECT USER AS connected_user,
       SYSDATE AS database_time
FROM dual;
~~~~

### Sample 6 (sql)

~~~~sql
SELECT employee_id, full_name, salary
FROM employees
ORDER BY salary DESC, employee_id;
~~~~

## 5. Filter, sort, and limit results

### Sample 1 (sql)

~~~~sql
SELECT employee_id, full_name, salary
FROM employees
WHERE salary >= 70000
  AND department_id IN (10, 20);
~~~~

### Sample 2 (sql)

~~~~sql
SELECT employee_id, full_name, hire_date
FROM employees
WHERE hire_date BETWEEN DATE '2022-01-01' AND DATE '2024-12-31'
  AND full_name LIKE 'A%';
~~~~

### Sample 3 (sql)

~~~~sql
SELECT employee_id, full_name
FROM employees
WHERE department_id IS NULL;
~~~~

### Sample 4 (sql)

~~~~sql
SELECT employee_id, full_name, department_id
FROM employees
ORDER BY department_id ASC NULLS LAST, employee_id;
~~~~

### Sample 5 (sql)

~~~~sql
SELECT employee_id, full_name, salary
FROM employees
ORDER BY salary DESC, employee_id
FETCH FIRST 10 ROWS ONLY;
~~~~

### Sample 6 (sql)

~~~~sql
SELECT employee_id, full_name
FROM employees
ORDER BY employee_id
OFFSET 20 ROWS FETCH NEXT 10 ROWS ONLY;
~~~~

## 6. SQL functions, NULL, and conversions

### Sample 1 (sql)

~~~~sql
SELECT employee_id,
       NVL(commission_pct, 0) AS commission_rate,
       COALESCE(department_id, 0) AS department_for_display
FROM employees;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT employee_id,
       CASE
         WHEN salary >= 100000 THEN 'Senior range'
         WHEN salary >= 60000 THEN 'Mid range'
         ELSE 'Entry range'
       END AS salary_band
FROM employees;
~~~~

### Sample 3 (sql)

~~~~sql
SELECT employee_id,
       TO_CHAR(hire_date, 'YYYY-MM-DD') AS hire_date_text,
       TO_CHAR(salary, 'FM9999990.00') AS salary_text
FROM employees;
~~~~

### Sample 4 (sql)

~~~~sql
SELECT TO_DATE('2024-01-15', 'YYYY-MM-DD') AS parsed_date
FROM dual;
~~~~

### Sample 5 (sql)

~~~~sql
SELECT employee_id, hire_date
FROM employees
WHERE hire_date >= DATE '2024-01-01'
  AND hire_date < DATE '2025-01-01';
~~~~

### Sample 6 (sql)

~~~~sql
SELECT employee_id,
       ADD_MONTHS(hire_date, 6) AS six_month_mark,
       TRUNC(hire_date) AS hire_day
FROM employees;
~~~~

### Sample 7 (sql)

~~~~sql
SELECT ROUND(AVG(salary), 2) AS average_salary
FROM employees;
~~~~

## 7. Joins and related tables

### Sample 1 (sql)

~~~~sql
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
~~~~

### Sample 2 (sql)

~~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
INNER JOIN departments d
  ON d.department_id = e.department_id
ORDER BY e.employee_id;
~~~~

### Sample 3 (sql)

~~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
  ON d.department_id = e.department_id
ORDER BY e.employee_id;
~~~~

### Sample 4 (sql)

~~~~sql
SELECT e.employee_id,
       e.full_name,
       d.department_name
FROM employees e
LEFT JOIN departments d
  ON d.department_id = e.department_id
 AND d.department_name = 'Engineering'
ORDER BY e.employee_id;
~~~~

## 8. Aggregates and grouping

### Sample 1 (sql)

~~~~sql
SELECT COUNT(*) AS employee_count,
       AVG(salary) AS average_salary,
       MIN(salary) AS lowest_salary,
       MAX(salary) AS highest_salary
FROM employees;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT department_id,
       COUNT(*) AS employee_count,
       ROUND(AVG(salary), 2) AS average_salary
FROM employees
GROUP BY department_id
ORDER BY department_id NULLS LAST;
~~~~

### Sample 3 (sql)

~~~~sql
SELECT department_id,
       COUNT(*) AS employee_count
FROM employees
WHERE salary >= 50000
GROUP BY department_id
HAVING COUNT(*) >= 2
ORDER BY employee_count DESC, department_id;
~~~~

### Sample 4 (sql)

~~~~sql
SELECT COUNT(DISTINCT department_id) AS assigned_departments
FROM employees;
~~~~

## 9. Subqueries, CTEs, and set operators

### Sample 1 (sql)

~~~~sql
SELECT employee_id, full_name, salary
FROM employees
WHERE salary > (
  SELECT AVG(salary)
  FROM employees
)
ORDER BY salary DESC, employee_id;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT e.employee_id, e.full_name
FROM employees e
WHERE EXISTS (
  SELECT 1
  FROM departments d
  WHERE d.department_id = e.department_id
    AND d.department_name = 'Engineering'
);
~~~~

### Sample 3 (sql)

~~~~sql
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
~~~~

### Sample 4 (sql)

~~~~sql
SELECT employee_id
FROM employees
WHERE salary >= 90000
UNION
SELECT employee_id
FROM employees
WHERE commission_pct IS NOT NULL
ORDER BY employee_id;
~~~~

## 10. Insert, update, delete, and transactions

### Sample 1 (sql)

~~~~sql
INSERT INTO employees (
  employee_id, full_name, department_id, salary, hire_date
)
VALUES (
  301, 'Meera Iyer', 20, 78000, DATE '2023-06-12'
);
~~~~

### Sample 2 (sql)

~~~~sql
UPDATE employees
SET salary = salary * 1.03
WHERE department_id = 20;
~~~~

### Sample 3 (sql)

~~~~sql
DELETE FROM employees
WHERE employee_id = 301;
~~~~

### Sample 4 (sql)

~~~~sql
SAVEPOINT before_salary_change;

UPDATE employees
SET salary = salary * 1.03
WHERE department_id = 20;

ROLLBACK TO before_salary_change;
~~~~

## 11. Keys, constraints, identity, and sequences

### Sample 1 (sql)

~~~~sql
ALTER TABLE departments
ADD CONSTRAINT departments_pk
PRIMARY KEY (department_id);

ALTER TABLE employees
ADD CONSTRAINT employees_pk
PRIMARY KEY (employee_id);
~~~~

### Sample 2 (sql)

~~~~sql
ALTER TABLE employees
ADD CONSTRAINT employees_department_fk
FOREIGN KEY (department_id)
REFERENCES departments (department_id);
~~~~

### Sample 3 (sql)

~~~~sql
ALTER TABLE employees
MODIFY full_name CONSTRAINT employees_name_nn NOT NULL;

ALTER TABLE employees
ADD CONSTRAINT employees_salary_ck
CHECK (salary >= 0);
~~~~

### Sample 4 (sql)

~~~~sql
CREATE TABLE practice_tasks (
  task_id NUMBER GENERATED BY DEFAULT AS IDENTITY,
  title VARCHAR2(120 CHAR) NOT NULL,
  CONSTRAINT practice_tasks_pk PRIMARY KEY (task_id)
);
~~~~

### Sample 5 (sql)

~~~~sql
CREATE SEQUENCE practice_task_seq
  START WITH 1
  INCREMENT BY 1;

INSERT INTO practice_tasks (task_id, title)
VALUES (practice_task_seq.NEXTVAL, 'Review keys');
~~~~

## 12. Views, indexes, and execution plans

### Sample 1 (sql)

~~~~sql
CREATE OR REPLACE VIEW employee_directory AS
SELECT employee_id, full_name, department_id
FROM employees;
~~~~

### Sample 2 (sql)

~~~~sql
SELECT employee_id, full_name
FROM employee_directory
WHERE department_id = 20
ORDER BY employee_id;
~~~~

### Sample 3 (sql)

~~~~sql
CREATE INDEX employees_department_salary_ix
ON employees (department_id, salary);
~~~~

### Sample 4 (sql)

~~~~sql
EXPLAIN PLAN FOR
SELECT employee_id, full_name
FROM employees
WHERE department_id = 20
ORDER BY salary;

SELECT *
FROM TABLE(DBMS_XPLAN.DISPLAY);
~~~~

## 13. PL/SQL blocks, variables, and control flow

### Sample 1 (sql)

~~~~sql
DECLARE
  v_message VARCHAR2(80) := 'Hello from PL/SQL';
BEGIN
  DBMS_OUTPUT.PUT_LINE(v_message);
END;
/
~~~~

### Sample 2 (sql)

~~~~sql
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
~~~~

### Sample 3 (sql)

~~~~sql
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
~~~~

### Sample 4 (sql)

~~~~sql
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
~~~~

### Sample 5 (sql)

~~~~sql
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
~~~~

## 14. PL/SQL cursors, routines, packages, and errors

### Sample 1 (sql)

~~~~sql
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
~~~~

### Sample 2 (sql)

~~~~sql
BEGIN
  UPDATE employees
  SET salary = salary * 1.02
  WHERE employee_id = 201;

  IF SQL%ROWCOUNT = 0 THEN
    RAISE_APPLICATION_ERROR(-20001, 'Employee was not found');
  END IF;
END;
/
~~~~

### Sample 3 (sql)

~~~~sql
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
~~~~

### Sample 4 (sql)

~~~~sql
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
~~~~

### Sample 5 (sql)

~~~~sql
CREATE OR REPLACE PACKAGE employee_api AS
  PROCEDURE raise_salary (
    p_employee_id IN employees.employee_id%TYPE,
    p_percent IN NUMBER
  );
END employee_api;
/
~~~~

### Sample 6 (sql)

~~~~sql
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
~~~~

## 15. Users, privileges, backup, and recovery

### Sample 1 (sql)

~~~~sql
CREATE ROLE reporting_reader;

GRANT SELECT ON app_owner.employees
TO reporting_reader;

GRANT reporting_reader TO report_user;
~~~~

### Sample 2 (sql)

~~~~sql
REVOKE SELECT ON app_owner.employees
FROM reporting_reader;
~~~~

### Sample 3 (sql)

~~~~sql
SELECT employee_id, full_name
FROM employees
WHERE employee_id = :employee_id
~~~~

## 16. Use Oracle Database from JavaScript

### Sample 1 (sh)

~~~~sh
npm init -y
npm install oracledb
~~~~

### Sample 2 (json)

~~~~json
{
  "name": "oracle-node-practice",
  "private": true,
  "type": "module"
}
~~~~

### Sample 3 (js)

~~~~js
import oracledb from 'oracledb'

const requiredSettings = [
  'DB_USER',
  'DB_PASSWORD',
  'DB_CONNECT_STRING'
]

for (const setting of requiredSettings) {
  if (!process.env[setting]) {
    throw new Error('Missing required setting: ' + setting)
  }
}

const pool = await oracledb.createPool({
  user: process.env.DB_USER,
  password: process.env.DB_PASSWORD,
  connectString: process.env.DB_CONNECT_STRING,
  poolMin: 1,
  poolMax: 5,
  poolIncrement: 1
})

try {
  const connection = await pool.getConnection()

  try {
    const result = await connection.execute(
      'SELECT employee_id, full_name FROM employees WHERE department_id = :departmentId',
      { departmentId: 20 },
      { outFormat: oracledb.OUT_FORMAT_OBJECT }
    )

    console.log(result.rows)
  } finally {
    await connection.close()
  }
} finally {
  await pool.close(10)
}
~~~~

### Sample 4 (js)

~~~~js
const departmentId = Number(process.argv[2])

if (!Number.isInteger(departmentId) || departmentId < 1) {
  throw new Error('Provide a positive department identifier')
}

const result = await connection.execute(
  'SELECT employee_id, full_name FROM employees WHERE department_id = :departmentId',
  { departmentId },
  { outFormat: oracledb.OUT_FORMAT_OBJECT }
)

console.log(result.rows)
~~~~

### Sample 5 (js)

~~~~js
try {
  const result = await connection.execute(
    'UPDATE employees SET salary = salary + :amount WHERE employee_id = :employeeId',
    { amount: 500, employeeId: 201 }
  )

  if (result.rowsAffected !== 1) {
    throw new Error('Expected exactly one employee row')
  }

  await connection.commit()
} catch (error) {
  await connection.rollback()
  throw error
}
~~~~

### Sample 6 (js)

~~~~js
const rows = [
  { employeeId: 201, amount: 100 },
  { employeeId: 202, amount: 150 }
]

const result = await connection.executeMany(
  'UPDATE employees SET salary = salary + :amount WHERE employee_id = :employeeId',
  rows
)

console.log('Rows changed:', result.rowsAffected)
await connection.commit()
~~~~

