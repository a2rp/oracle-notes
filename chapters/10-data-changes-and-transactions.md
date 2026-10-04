# 10. Insert, update, delete, and transactions

[Back to notes index](../README.md)

| [Previous: Subqueries, CTEs, and set operators](./09-subqueries-ctes-and-set-operators.md) | [Notes index](../README.md) | [Next: Keys, constraints, identity, and sequences](./11-keys-constraints-identity-and-sequences.md) |
| --- | --- | --- |

## Add rows with INSERT

List target columns so the statement remains clear if the table changes:

~~~sql
INSERT INTO employees (
  employee_id, full_name, department_id, salary, hire_date
)
VALUES (
  301, 'Meera Iyer', 20, 78000, DATE '2023-06-12'
);
~~~

A successful insert changes the current transaction. It becomes durable after **COMMIT**, unless the client or connection uses a different transaction mode.

## Change selected rows with UPDATE

Use a selective condition and verify the affected rows:

~~~sql
UPDATE employees
SET salary = salary * 1.03
WHERE department_id = 20;
~~~

Before a broad update, run a matching SELECT with the same WHERE condition. If the condition is missing, every row can be updated.

## Remove selected rows with DELETE

~~~sql
DELETE FROM employees
WHERE employee_id = 301;
~~~

A DELETE without a WHERE condition removes every row from the table. Review the predicate and affected-row count before committing a destructive change.

## Commit, roll back, and use a savepoint

A transaction groups related DML changes. **COMMIT** makes the current transaction durable. **ROLLBACK** discards its uncommitted changes.

~~~sql
SAVEPOINT before_salary_change;

UPDATE employees
SET salary = salary * 1.03
WHERE department_id = 20;

ROLLBACK TO before_salary_change;
~~~

A savepoint lets the session undo work after that marker while keeping earlier transaction changes. The example returns the salary update to its prior value without ending the full transaction.

## Understand Oracle transaction behavior

Oracle provides statement-level read consistency and manages row locks for changes. A transaction that changes a row holds its lock until commit or rollback. Keep transactions short so other sessions are not blocked longer than necessary.

Oracle issues implicit commits around DDL statements such as **CREATE TABLE** and **ALTER TABLE**. Do not expect DDL to participate in the same rollback unit as ordinary DML. Perform schema changes separately from business-data changes.

## Practice safely

In a disposable schema, select the rows for one department, update a small value, inspect it, and roll back. Repeat the change and commit it. Test a delete against a single known identifier. Do not practice destructive statements on production data.

## Check what you learned

1. Why should an INSERT list its target columns?
2. What can happen when UPDATE has no WHERE condition?
3. What can happen when DELETE has no WHERE condition?
4. What does COMMIT do?
5. What does ROLLBACK do?
6. What does a savepoint allow a session to undo?
7. How long can a transaction hold a changed row's lock?
8. Why should DDL be handled separately from a DML rollback plan?

## References

- [INSERT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/INSERT.html)
- [UPDATE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/UPDATE.html)
- [DELETE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/DELETE.html)
- [COMMIT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COMMIT.html)
- [Oracle Database concepts: transactions](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/transactions.html)