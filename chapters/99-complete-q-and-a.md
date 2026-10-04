# Complete questions and answers

[Back to notes index](../README.md)

| [Previous: All code samples](./98-all-code-samples.md) | [Notes index](../README.md) | Next: End of notes |
| --- | --- | --- |

This appendix answers the review questions from every core chapter. Use the answers to check understanding, then return to each chapter example for context.

## 1. Oracle Database and relational foundations

1. **Question:** What does a relational table contain?
   **Answer:** A relational table organizes named columns and rows of related values.

2. **Question:** How does an Oracle database differ from an Oracle instance?
   **Answer:** The database is the persistent files; the instance is the running memory structures and processes that manage access to those files.

3. **Question:** What is a pluggable database used for?
   **Answer:** A PDB provides a separate pluggable database environment inside a multitenant container database.

4. **Question:** What does a schema describe?
   **Answer:** A schema is the collection of database objects owned by a database user.

5. **Question:** How does a tablespace relate to datafiles?
   **Answer:** A tablespace is a logical storage container backed by one or more physical datafiles.

6. **Question:** What kinds of work are described with SQL?
   **Answer:** SQL describes data operations such as reading, changing, or defining database objects.

7. **Question:** What procedural features does PL/SQL add?
   **Answer:** PL/SQL adds variables, conditions, loops, exception handling, and stored routines.

8. **Question:** What does the USER_TABLES view list?
   **Answer:** USER_TABLES lists tables owned by the current database user.

## 2. Connect with SQL Developer and SQLcl

1. **Question:** What is the difference between a SQL client and the database server?
   **Answer:** A SQL client sends commands to a database server and displays results; it does not itself provide the database service.

2. **Question:** Which connection detail identifies the database service?
   **Answer:** The service name identifies which database service, often a PDB service, receives the session.

3. **Question:** What does a connection label in SQL Developer change?
   **Answer:** It changes only the saved label displayed inside the client.

4. **Question:** Why should passwords stay out of saved commands and source files?
   **Answer:** A saved password can leak through source control, command history, or screenshots.

5. **Question:** What does the user and container query confirm?
   **Answer:** It confirms the authenticated account and the container that received the session.

6. **Question:** Is SHOW USER a SQL statement sent to the database?
   **Answer:** SHOW USER is interpreted by a SQL*Plus-style client rather than sent as a SQL statement.

7. **Question:** What character normally ends a SQL statement in a worksheet?
   **Answer:** A semicolon normally terminates a SQL statement in a worksheet.

8. **Question:** What does the slash do in a SQL*Plus-style client after a PL/SQL block?
   **Answer:** The slash asks a SQL*Plus-compatible client to submit the current PL/SQL block buffer.

## 3. Create schemas, tables, and Oracle data types

1. **Question:** What kinds of objects belong to a schema?
   **Answer:** A schema owns database objects such as tables, views, indexes, sequences, and routines.

2. **Question:** Which schema owns a table created without a schema prefix?
   **Answer:** The user connected to the session owns an unqualified table created in that session.

3. **Question:** How does Oracle store ordinary unquoted identifiers?
   **Answer:** Ordinary unquoted names are stored in uppercase in Oracle's data dictionary.

4. **Question:** When is VARCHAR2 suitable?
   **Answer:** VARCHAR2 stores variable-length character text.

5. **Question:** What is the difference between NUMBER precision and scale?
   **Answer:** Precision limits the number of significant digits and scale sets the digits to the right of the decimal point.

6. **Question:** Does the Oracle DATE type store a time component?
   **Answer:** Yes. Oracle DATE stores a calendar date and a time down to seconds.

7. **Question:** Which types can hold large character and binary values?
   **Answer:** CLOB holds large character data, and BLOB holds large binary data.

8. **Question:** Why is an Oracle date literal useful in a script?
   **Answer:** It specifies a stable date format without relying on the current session's display format.

## 4. Select rows and shape results

1. **Question:** What does SELECT do?
   **Answer:** SELECT reads rows and expressions from database objects.

2. **Question:** Why should a query list only the columns its caller needs?
   **Answer:** Selecting only needed columns makes the result clearer and avoids retrieving unnecessary values.

3. **Question:** What does a column alias change?
   **Answer:** An alias changes the heading or name of a query result expression, not the stored table definition.

4. **Question:** What does DISTINCT remove?
   **Answer:** DISTINCT removes duplicate combinations from the selected result rows.

5. **Question:** Why should DISTINCT not be used to hide join mistakes?
   **Answer:** Unexpected duplicates may come from an incorrect join or data relationship and should be understood.

6. **Question:** What is the DUAL table useful for?
   **Answer:** DUAL can evaluate expressions or functions that do not need application table rows.

7. **Question:** Does a table guarantee row order without ORDER BY?
   **Answer:** No. The database does not guarantee row order without ORDER BY.

8. **Question:** Why add a unique tie-breaker to ORDER BY?
   **Answer:** A unique tie-breaker defines the order of rows whose primary sort values are equal.

## 5. Filter, sort, and limit results

1. **Question:** What does a WHERE clause do?
   **Answer:** WHERE keeps only rows whose condition evaluates to true.

2. **Question:** When is IN useful?
   **Answer:** IN checks whether a value matches one of a listed set of values.

3. **Question:** Is BETWEEN inclusive of both endpoints?
   **Answer:** Yes. BETWEEN includes both its lower and upper endpoints.

4. **Question:** What does NULL represent?
   **Answer:** NULL represents a missing or unknown value.

5. **Question:** Why does equality comparison not find NULL values?
   **Answer:** Comparisons with NULL do not evaluate to true; use IS NULL or IS NOT NULL.

6. **Question:** What does the percent wildcard match in a LIKE pattern?
   **Answer:** The percent wildcard matches zero or more characters.

7. **Question:** Why should null placement be explicit when it matters?
   **Answer:** Explicit null placement makes result ordering predictable for the reader.

8. **Question:** Why does a row limit need a stable ORDER BY?
   **Answer:** Without a stable order, pages can contain inconsistent or repeated rows.

## 6. SQL functions, NULL, and conversions

1. **Question:** What value does NVL return when its first argument is NULL?
   **Answer:** NVL returns its second argument when its first argument is NULL.

2. **Question:** How does COALESCE choose its result?
   **Answer:** COALESCE returns the first non-NULL expression in its argument list.

3. **Question:** Why can replacing NULL with zero misrepresent data?
   **Answer:** Zero may be a real measured value, so using it for missing data can change the meaning.

4. **Question:** How does a searched CASE expression choose a value?
   **Answer:** A searched CASE checks conditions in order and returns the result for the first true condition.

5. **Question:** What is TO_CHAR used for?
   **Answer:** TO_CHAR converts a date or number to formatted text.

6. **Question:** Why should an explicit format model be used with TO_DATE?
   **Answer:** An explicit mask avoids relying on session defaults when interpreting a date string.

7. **Question:** Why is a half-open date range useful for columns with time values?
   **Answer:** It includes every time from the start value up to, but not including, the next boundary.

8. **Question:** How can a function on a filtered column affect an index?
   **Answer:** Applying a function may stop Oracle from using an ordinary index access path as expected.

## 7. Joins and related tables

1. **Question:** What does an INNER JOIN return?
   **Answer:** An INNER JOIN returns row combinations whose join condition matches.

2. **Question:** What does a LEFT JOIN preserve?
   **Answer:** A LEFT JOIN keeps every row from its left input and adds matching right-side values.

3. **Question:** Which side's columns become NULL when a left-side row has no match?
   **Answer:** Right-side columns are NULL when a left-side row has no match.

4. **Question:** Why qualify repeated column names with table aliases?
   **Answer:** Aliases clarify which table supplies a column, especially when names overlap.

5. **Question:** How can a predicate in WHERE change a left join's result?
   **Answer:** A WHERE condition on the right side can remove NULL-extended rows and change the left join result.

6. **Question:** Why can one parent row appear multiple times after a join?
   **Answer:** A parent row appears once for each matching child row in a one-to-many relationship.

7. **Question:** Why should duplicate results be investigated before adding DISTINCT?
   **Answer:** DISTINCT can hide an incorrect join instead of fixing its condition or relationship.

8. **Question:** What is a self-join?
   **Answer:** A self-join uses one table twice under separate aliases to relate its rows.

## 8. Aggregates and grouping

1. **Question:** What does an aggregate function calculate?
   **Answer:** An aggregate function summarizes values across one or more rows.

2. **Question:** What is the difference between COUNT(*) and COUNT(column)?
   **Answer:** COUNT(*) counts rows; COUNT(column) counts only rows where that column is not NULL.

3. **Question:** How do most aggregate functions treat NULL inputs?
   **Answer:** Most aggregates ignore NULL inputs.

4. **Question:** What does GROUP BY produce?
   **Answer:** GROUP BY produces one result group for each distinct combination of grouping expressions.

5. **Question:** What happens to rows with a NULL grouping value?
   **Answer:** Rows with NULL in a grouping value form a group with a NULL key.

6. **Question:** When should WHERE be used?
   **Answer:** WHERE filters individual input rows before grouping.

7. **Question:** When should HAVING be used?
   **Answer:** HAVING filters completed groups, often by an aggregate result.

8. **Question:** What values does COUNT(DISTINCT column) count?
   **Answer:** COUNT(DISTINCT column) counts unique non-NULL column values.

## 9. Subqueries, CTEs, and set operators

1. **Question:** What does a scalar subquery return?
   **Answer:** A scalar subquery returns one value in one column from one row.

2. **Question:** What does EXISTS test?
   **Answer:** EXISTS tests whether the subquery returns at least one row.

3. **Question:** Why is a subquery correlated when it refers to an outer query alias?
   **Answer:** It is correlated when its predicate uses a value from the current outer-query row.

4. **Question:** What is a CTE?
   **Answer:** A CTE is a named subquery declared in a WITH clause for one statement.

5. **Question:** Does a CTE create a permanent table?
   **Answer:** No. It exists as part of the query statement and does not automatically store rows.

6. **Question:** What must be compatible between the branches of a set operator?
   **Answer:** The branches must return the same number of columns with compatible data types.

7. **Question:** How does UNION differ from UNION ALL?
   **Answer:** UNION removes duplicate result rows; UNION ALL keeps duplicates.

8. **Question:** Which Oracle set operator subtracts the second result from the first?
   **Answer:** MINUS returns rows from the first result that are absent from the second.

## 10. Insert, update, delete, and transactions

1. **Question:** Why should an INSERT list its target columns?
   **Answer:** Listing target columns makes the insert's value mapping explicit and resilient to table changes.

2. **Question:** What can happen when UPDATE has no WHERE condition?
   **Answer:** Every row may be changed if its predicate is omitted.

3. **Question:** What can happen when DELETE has no WHERE condition?
   **Answer:** Every row may be removed if its predicate is omitted.

4. **Question:** What does COMMIT do?
   **Answer:** COMMIT makes the current transaction's changes durable.

5. **Question:** What does ROLLBACK do?
   **Answer:** ROLLBACK discards uncommitted changes in the current transaction.

6. **Question:** What does a savepoint allow a session to undo?
   **Answer:** ROLLBACK TO a savepoint undoes work performed after that marker without ending the whole transaction.

7. **Question:** How long can a transaction hold a changed row's lock?
   **Answer:** A transaction can hold a changed row's lock until it commits or rolls back.

8. **Question:** Why should DDL be handled separately from a DML rollback plan?
   **Answer:** Oracle commits around DDL, so schema operations do not share the normal DML rollback unit.

## 11. Keys, constraints, identity, and sequences

1. **Question:** What does a primary key guarantee?
   **Answer:** A user is an account; its schema is the set of objects that the account owns.

2. **Question:** What does a foreign key validate?
   **Answer:** A system privilege grants a class of database actions; an object privilege grants actions on a specific object.

3. **Question:** What must be true of a referenced foreign key column?
   **Answer:** Roles group grants so access can be managed and reviewed together.

4. **Question:** What is the difference between NOT NULL and CHECK?
   **Answer:** Narrow access limits accidental changes and the damage from compromised credentials.

5. **Question:** Why should a constraint have a clear name?
   **Answer:** Binding sends values separately from SQL text so data is not parsed as executable SQL.

6. **Question:** What is an identity column used for?
   **Answer:** Bind placeholders represent data values, not SQL identifiers such as table or column names.

7. **Question:** What does a sequence NEXTVAL return?
   **Answer:** RMAN handles Oracle database file backup and recovery operations.

8. **Question:** Why are sequence values not a reliable gap-free row count?
   **Answer:** Data Pump exports and imports logical objects and rows rather than replacing a physical database backup.

## 12. Views, indexes, and execution plans

1. **Question:** What does a regular view store?
   **Answer:** A regular view stores its query definition and presents its results when queried.

2. **Question:** How does a materialized view differ from a regular view?
   **Answer:** A materialized view stores query results and needs a refresh strategy to keep them current.

3. **Question:** What can an index help Oracle do?
   **Answer:** An index can help locate rows for a query without scanning every table block.

4. **Question:** Why does a composite index's column order matter?
   **Answer:** Its leading column determines which lookup and ordering patterns the composite index can support well.

5. **Question:** What costs do indexes add to data changes?
   **Answer:** Indexes consume storage and must be maintained when rows change.

6. **Question:** What does an execution plan describe?
   **Answer:** An execution plan describes the operations Oracle expects to use to execute a statement.

7. **Question:** Does a table scan always mean a query is poorly designed?
   **Answer:** No. A full scan can be appropriate when many rows are needed or a table is small.

8. **Question:** Why can an estimated plan differ from the plan used during execution?
   **Answer:** Runtime binds, statistics, settings, and conditions can lead to a plan different from an estimate.

## 13. PL/SQL blocks, variables, and control flow

1. **Question:** Which sections can appear in an anonymous PL/SQL block?
   **Answer:** A block can have DECLARE, BEGIN, and EXCEPTION sections, with some sections optional.

2. **Question:** Which section must be present?
   **Answer:** The BEGIN executable section is required.

3. **Question:** What does %TYPE copy from a table column?
   **Answer:** %TYPE declares a variable using a table column's data type.

4. **Question:** What does %ROWTYPE describe?
   **Answer:** %ROWTYPE declares a record whose fields correspond to a table row or query row.

5. **Question:** What happens when SELECT INTO returns no rows?
   **Answer:** NO_DATA_FOUND is raised.

6. **Question:** What happens when SELECT INTO returns multiple rows?
   **Answer:** TOO_MANY_ROWS is raised.

7. **Question:** What does the slash do after a block in a SQL*Plus-style client?
   **Answer:** It tells a SQL*Plus-style client to submit the completed block buffer.

8. **Question:** Why prefer set-based SQL over an unnecessary procedural loop?
   **Answer:** Set-based SQL lets the database operate on a group of rows without an unnecessary PL/SQL loop.

## 14. PL/SQL cursors, routines, packages, and errors

1. **Question:** What work does a cursor FOR loop handle automatically?
   **Answer:** A cursor FOR loop opens the query, fetches each row, and closes the cursor automatically.

2. **Question:** When is set-based SQL preferable to row-by-row processing?
   **Answer:** Set-based SQL is simpler and often more efficient when every row can receive the same relational operation.

3. **Question:** What does SQL%ROWCOUNT report?
   **Answer:** SQL%ROWCOUNT reports how many rows the most recent DML statement affected.

4. **Question:** Why might a procedure raise an application error for a missing row?
   **Answer:** It lets the routine distinguish a successful update from an identifier that matched no employee.

5. **Question:** What distinguishes a procedure from a function?
   **Answer:** A procedure performs an operation; a function is defined to return a value.

6. **Question:** What does a function declare and return?
   **Answer:** A function declares a return type and must return a value on a successful execution path.

7. **Question:** What does a package specification contain?
   **Answer:** The package specification declares the public names and signatures callers can use.

8. **Question:** Which package contents can remain private in the body?
   **Answer:** Private helper routines and state may be defined in the package body without appearing in its specification.

## 15. Users, privileges, backup, and recovery

1. **Question:** What is the relationship between a database user and its schema?
   **Answer:** A database user is an account and owns a correspondingly named schema of database objects.

2. **Question:** How does a system privilege differ from an object privilege?
   **Answer:** A system privilege permits a category of actions; an object privilege permits actions on one named object.

3. **Question:** Why are roles useful?
   **Answer:** A role groups grants so administrators can assign and review related access in one place.

4. **Question:** Why should an application account receive only required privileges?
   **Answer:** Least privilege reduces the effects of mistakes or compromised application credentials.

5. **Question:** How do bind variables reduce SQL injection risk?
   **Answer:** Bind variables keep user data out of the parsed SQL statement text.

6. **Question:** Why can table names not be replaced with ordinary bind variables?
   **Answer:** SQL identifiers are part of statement structure; bind variables substitute data values only.

7. **Question:** What kind of database work is RMAN designed for?
   **Answer:** RMAN is designed for physical database backup, restore, and recovery operations.

8. **Question:** How does a Data Pump export differ from a physical database backup?
   **Answer:** Data Pump exports and imports logical schemas and data, while a physical backup protects database files for recovery.

## 16. Use Oracle Database from JavaScript

1. **Question:** Which driver connects JavaScript applications to Oracle Database?
   **Answer:** node-oracledb is Oracle's Node.js driver for connecting JavaScript applications to Oracle Database.

2. **Question:** What is the purpose of a connection pool?
   **Answer:** A pool reuses a bounded set of database connections instead of creating a fresh connection for every request.

3. **Question:** Why read connection settings from the environment?
   **Answer:** Environment settings keep credentials and deployment-specific connection details out of source files.

4. **Question:** How does closing a pooled connection differ from closing the pool?
   **Answer:** Closing a connection returns it to the pool; closing the pool shuts down the pool and its resources.

5. **Question:** Why should application values use bind variables?
   **Answer:** Bind variables separate values from SQL text, reducing injection risk and supporting statement reuse.

6. **Question:** Can a bind variable stand in for a table name?
   **Answer:** No. A table name is a SQL identifier, not a data value.

7. **Question:** Why should driver SQL omit SQL worksheet terminators?
   **Answer:** The driver sends statement text to the database; semicolon and slash are client submission commands.

8. **Question:** When should an application commit or roll back a transaction?
   **Answer:** Commit after all intended related changes succeed; roll back when the operation should be discarded or fails.

