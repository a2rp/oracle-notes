# 16. Use Oracle Database from JavaScript

[Back to notes index](../README.md)

| [Previous: Users, privileges, backup, and recovery](./15-security-backup-and-recovery.md) | [Notes index](../README.md) | [Next: All code samples](./98-all-code-samples.md) |
| --- | --- | --- |

## Add the Oracle driver to a Node.js project

The Oracle-maintained **node-oracledb** driver lets a JavaScript application connect to Oracle Database. Start with the project's installation guide and compatibility information for the Node.js and database versions in use.

~~~sh
npm init -y
npm install oracledb
~~~

Use ESM in the project manifest when writing the examples with **import**:

~~~json
{
  "name": "oracle-node-practice",
  "private": true,
  "type": "module"
}
~~~

The driver supports a Thin mode and an optional Thick mode that uses Oracle Client libraries. Choose a mode based on the needed features and the official compatibility documentation.

## Keep connection settings outside source code

Store the database username, password, and connection string in the process environment. For local work, a Node.js release that supports environment files can load a local **.env** file. Keep that file out of Git and use placeholder values in **.env.example**.

A connection string often identifies the host, listener port, and service name. Use the exact values from the database administrator or approved installation guide.

## Create a connection pool

A pool reuses a bounded set of database connections:

~~~js
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
~~~

The example validates settings before creating a pool, acquires one connection, and releases it in a **finally** block. Closing a pooled connection returns it to the pool. Close the pool during application shutdown.

## Bind values instead of building SQL strings

Bind variables keep values separate from SQL text:

~~~js
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
~~~

Do not concatenate input into SQL. Bind variables represent data values, not table or column names. If a table or sort column must vary, select it from a fixed allowlist and construct only that validated identifier.

**node-oracledb** expects SQL statement text without a trailing semicolon or SQL*Plus slash. Those terminators belong to interactive SQL clients, not the driver statement string.

## Commit or roll back a data change

Keep related DML in one transaction and make the outcome explicit:

~~~js
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
~~~

Do not commit each statement if several changes must succeed together. Release the connection in **finally** even when the statement or commit fails.

## Use executeMany for repeated data

For a batch of similar DML operations, **executeMany** can send multiple bind sets efficiently:

~~~js
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
~~~

Validate each item before sending the batch. Decide what the application should do when one row does not match or a statement fails, and roll back when the batch is part of a larger all-or-nothing operation.

## Practice

Connect to a disposable Oracle schema. Query one department with a bind variable, then update one employee inside a transaction and roll it back. Repeat the update and commit it. Confirm connections are returned to the pool and the pool closes during shutdown.

## Check what you learned

1. Which driver connects JavaScript applications to Oracle Database?
2. What is the purpose of a connection pool?
3. Why read connection settings from the environment?
4. How does closing a pooled connection differ from closing the pool?
5. Why should application values use bind variables?
6. Can a bind variable stand in for a table name?
7. Why should driver SQL omit SQL worksheet terminators?
8. When should an application commit or roll back a transaction?

## References

- [node-oracledb documentation](https://oracle.github.io/node-oracledb/)
- [node-oracledb installation](https://oracle.github.io/node-oracledb/INSTALL.html)
- [node-oracledb SQL execution](https://node-oracledb.readthedocs.io/en/latest/user_guide/sql_execution.html)
- [node-oracledb bind variables](https://node-oracledb.readthedocs.io/en/latest/user_guide/bind.html)
- [node-oracledb connection pooling](https://node-oracledb.readthedocs.io/en/latest/user_guide/connection_handling.html#connection-pooling)