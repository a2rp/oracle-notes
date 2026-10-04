# 15. Users, privileges, backup, and recovery

[Back to notes index](../README.md)

| [Previous: PL/SQL cursors, routines, packages, and errors](./14-plsql-routines-cursors-and-errors.md) | [Notes index](../README.md) | [Next: Use Oracle Database from JavaScript](./16-oracle-database-with-javascript.md) |
| --- | --- | --- |

## Separate accounts and responsibilities

A database user is an account that can connect and own a schema. Use separate accounts for administration, application runtime, reporting, and development rather than sharing one powerful account.

A database administrator creates users and grants connection permissions according to the installation's policy. Do not practice account administration on a shared or production database.

## Grant only the privileges a task needs

System privileges allow classes of actions. Object privileges allow actions on specific objects. A role groups privileges so they can be assigned and reviewed as a unit.

~~~sql
CREATE ROLE reporting_reader;

GRANT SELECT ON app_owner.employees
TO reporting_reader;

GRANT reporting_reader TO report_user;
~~~

The example grants read access to one table through a role. Replace schema and account names with approved values. Avoid broad privileges such as unrestricted access to every table when a narrower grant is sufficient.

Remove access that is no longer needed:

~~~sql
REVOKE SELECT ON app_owner.employees
FROM reporting_reader;
~~~

Review grants periodically and use separate accounts for separate responsibilities.

## Keep application SQL safe

Bind variables keep application data separate from the SQL statement text:

~~~sql
SELECT employee_id, full_name
FROM employees
WHERE employee_id = :employee_id
~~~

Application code supplies the value for **:employee_id** separately. Do not concatenate untrusted values into SQL. Bind variables are for data values; table names and column names require a separate allowlist and careful validation if they must vary.

## Distinguish backup tools

A backup is useful only if the required data can be restored and the recovery steps are understood.

- RMAN performs Oracle database backup and recovery operations for database files and archived redo.
- Data Pump export and import move logical database objects and data between environments.
- A copied SQL script is not a replacement for a tested database backup.

Choose a backup schedule and recovery point based on business requirements. Protect backup files and credentials as carefully as the database itself.

## Practice recovery planning

In a disposable environment, identify who owns the RMAN configuration, where backups are stored, and how a restore test is performed. For a logical migration, identify the Data Pump export scope and the target schema. Record the validation checks that prove the restored or imported data is usable.

Do not run restore, overwrite, or account-management commands against a shared database as an experiment.

## Check what you learned

1. What is the relationship between a database user and its schema?
2. How does a system privilege differ from an object privilege?
3. Why are roles useful?
4. Why should an application account receive only required privileges?
5. How do bind variables reduce SQL injection risk?
6. Why can table names not be replaced with ordinary bind variables?
7. What kind of database work is RMAN designed for?
8. How does a Data Pump export differ from a physical database backup?

## References

- [Oracle Database security guide](https://docs.oracle.com/en/database/oracle/oracle-database/23/dbseg/)
- [GRANT](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/GRANT.html)
- [REVOKE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/REVOKE.html)
- [RMAN backup and recovery](https://docs.oracle.com/en/database/oracle/oracle-database/23/bradv/)
- [Data Pump export and import](https://docs.oracle.com/en/database/oracle/oracle-database/23/sutil/)