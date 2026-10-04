# 1. Oracle Database and relational foundations

[Back to notes index](../README.md)

| [Previous: Notes index](../README.md) | [Notes index](../README.md) | [Next: Connect with SQL Developer and SQLcl](./02-connect-with-sql-developer-and-sqlcl.md) |
| --- | --- | --- |

## What a relational database stores

A relational database stores related facts in tables. A table has named columns and rows. Each column has a data type and represents one kind of value. A primary key identifies a row, and a foreign key can connect a row to another table.

For a small learning example, a **departments** table might contain:

| DEPARTMENT_ID | DEPARTMENT_NAME |
| ---: | --- |
| 10 | Sales |
| 20 | Engineering |

The identifier is more reliable than using a department name as a relationship because names can change and may not be unique.

## Database and instance

An Oracle database is the set of persistent database files managed by Oracle. An Oracle instance is the running memory structures and background processes that manage access to those files.

This distinction helps explain why starting an instance and opening a database are related but different administration operations. As a SQL learner, connect to an already configured practice database and avoid startup or shutdown commands unless you are working in a disposable local environment.

## Container databases, pluggable databases, and schemas

In a multitenant installation, a container database can hold one or more pluggable databases. A PDB provides a database environment that applications and users can connect to. The connection service usually identifies which PDB receives the session.

A schema is the collection of database objects owned by a database user. Tables, views, sequences, and PL/SQL routines belong to a schema. The terms schema and user are closely related in Oracle because a user owns a same-named schema, but a schema describes objects while a user is an account that can authenticate and receive privileges.

Check the container for the current session:

~~~sql
SELECT SYS_CONTEXT('USERENV', 'CON_NAME') AS container_name
FROM dual;
~~~

The exact container name depends on the database installation and service used for the connection.

## How objects use storage

A tablespace is a logical storage container. Datafiles provide physical storage for tablespaces. Oracle manages the smaller blocks and extents used to store segments such as tables and indexes.

Usually, application developers choose the table design and constraints while database administrators manage storage and capacity. Understand the terms so that an error about a tablespace or datafile has context, but do not change storage settings in a shared environment without an operational reason.

## SQL and PL/SQL

SQL describes data operations such as reading rows, changing values, and defining tables. PL/SQL adds procedural features such as variables, conditions, loops, exception handling, and stored routines.

A SQL client sends statements to the database. Some client tools use extra commands to control the client or format results. For example, the slash used to submit a PL/SQL block in SQL*Plus is a client command, not part of the PL/SQL block itself.

## Inspect your own schema

After connecting as a practice user, list tables owned by the current schema:

~~~sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
~~~

The **USER_** data dictionary views describe objects owned by the current user. Other dictionary view families provide information about accessible objects or the whole database, subject to permissions.

## Practice

Connect to a disposable training PDB. Run the container query and list the tables owned by your user. Write down the database service and schema user shown by your environment, then distinguish the instance, PDB, user, and schema in your own words.

## Check what you learned

1. What does a relational table contain?
2. How does an Oracle database differ from an Oracle instance?
3. What is a pluggable database used for?
4. What does a schema describe?
5. How does a tablespace relate to datafiles?
6. What kinds of work are described with SQL?
7. What procedural features does PL/SQL add?
8. What does the USER_TABLES view list?

## References

- [Oracle Database concepts](https://docs.oracle.com/en/database/oracle/oracle-database/23/cncpt/)
- [Introduction to the multitenant architecture](https://docs.oracle.com/en/database/oracle/oracle-database/23/multi/introduction-to-the-multitenant-architecture.html)
- [Oracle SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/)
- [Oracle PL/SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/)