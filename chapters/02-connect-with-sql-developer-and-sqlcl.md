# 2. Connect with SQL Developer and SQLcl

[Back to notes index](../README.md)

| [Previous: Oracle Database and relational foundations](./01-oracle-database-and-relational-foundations.md) | [Notes index](../README.md) | [Next: Create schemas, tables, and Oracle data types](./03-schemas-tables-and-oracle-data-types.md) |
| --- | --- | --- |

## Choose a SQL client

Oracle SQL Developer is a graphical application for connecting to Oracle Database, writing SQL and PL/SQL, browsing schema objects, and viewing query results.

SQLcl is a command-line client for interactive work and scripts. SQL*Plus is another command-line client that is commonly available in Oracle environments. Choose the client already supported by your practice database and follow its installation instructions.

A SQL client is separate from the database server. Installing a client does not itself create a database.

## Collect the connection details

A connection usually needs:

- A hostname or IP address
- A listener port
- A service name, often for a PDB
- A database username and password
- Network or wallet settings when required by the environment

For example, a local training database might provide a service name such as **FREEPDB1**. Use the service name given by the administrator or installation guide rather than guessing it.

Keep passwords out of source code, shell history, screenshots, and committed files. When a client can prompt for the password, use the prompt instead of embedding credentials in a saved command.

## Create a connection in SQL Developer

Create a new database connection and fill in the connection name, username, password, host, port, and service name. Test the connection before saving it. The connection name is only a label in the client; it does not change the database user or service.

Open a SQL Worksheet for the connection and run:

~~~sql
SELECT USER AS connected_user,
       SYS_CONTEXT('USERENV', 'CON_NAME') AS container_name
FROM dual;
~~~

The result confirms which account and container received the session. If the connection test fails, check the host, port, service, network access, and whether the database listener is running.

## Connect from a command-line client

Start SQLcl or SQL*Plus using the connection syntax documented for the installed client. When possible, let the client prompt for the password. The same connection values used in SQL Developer are needed by the terminal client.

After connecting, confirm the user:

~~~sql
SHOW USER
~~~

**SHOW USER** is a SQL*Plus-style client command. It is not a SQL statement sent to the database. Client commands can differ, so check the help for the exact tool in use.

## Run SQL and PL/SQL

A SQL statement in a worksheet normally ends with a semicolon:

~~~sql
SELECT table_name
FROM user_tables
ORDER BY table_name;
~~~

A PL/SQL block is commonly submitted from SQL*Plus-compatible tools with a slash on a new line:

~~~sql
BEGIN
  DBMS_OUTPUT.PUT_LINE('Connected to Oracle');
END;
/
~~~

The slash is a client instruction that submits the current buffer. In SQL Developer, use the command for running a statement or the command for running a script according to how the block is written. Enable DBMS Output in the client when you want to see text written with **DBMS_OUTPUT.PUT_LINE**.

## Practice

Connect to a local or approved practice database. Run the user and container query, then list your own tables. Disconnect and reconnect using a second available client if you have one, and confirm both clients reach the same service and schema.

## Check what you learned

1. What is the difference between a SQL client and the database server?
2. Which connection detail identifies the database service?
3. What does a connection label in SQL Developer change?
4. Why should passwords stay out of saved commands and source files?
5. What does the user and container query confirm?
6. Is SHOW USER a SQL statement sent to the database?
7. What character normally ends a SQL statement in a worksheet?
8. What does the slash do in a SQL*Plus-style client after a PL/SQL block?

## References

- [Oracle SQL Developer documentation](https://docs.oracle.com/en/database/oracle/sql-developer/)
- [SQLcl documentation](https://docs.oracle.com/en/database/oracle/sqlcl/)
- [SQL*Plus User's Guide](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqpug/)
- [Oracle Database connection concepts](https://docs.oracle.com/en/database/oracle/oracle-database/23/netag/)