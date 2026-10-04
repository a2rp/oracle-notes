# Oracle Database Study Notes

These are my personal study notes from learning Oracle Database and working with SQL and PL/SQL. I am collecting the database concepts, query patterns, procedural features, and application examples I want to understand and revisit while studying and working.

## About these notes

The chapters begin with relational foundations and database tools, then move through Oracle SQL, data changes, constraints, query plans, PL/SQL, security, recovery, and JavaScript application access. Each chapter explains why a concept matters, includes practical Oracle SQL, PL/SQL, or JavaScript examples, and ends with review questions. The final appendices collect all chapter code samples and provide answers to every review question.

SQL examples use Oracle syntax. Application examples use JavaScript with the Oracle-maintained node-oracledb driver. Oracle Database editions, installed sample schemas, client tools, and driver requirements can vary, so check the official documentation for the environment you use.

## Core topics

- Understand Oracle Database, schemas, tables, and relational data
- Connect with SQL Developer, SQLcl, or SQL*Plus
- Create tables with suitable Oracle data types and constraints
- Read and shape results with SELECT, filters, ordering, and row limiting
- Use Oracle SQL functions, NULL rules, joins, aggregation, and subqueries
- Change data safely with transactions and constraints
- Read execution plans and understand indexes
- Write PL/SQL blocks, cursors, procedures, functions, and packages
- Apply least-privilege access and understand backup and recovery
- Connect a JavaScript application with bound SQL and pooled connections

## How I use these notes

I run statements against a disposable practice schema and inspect each result before moving on. I keep destructive statements away from important databases, use bind variables for application data, and use review questions to check that I can explain each concept in my own words.

## Chapters
01. [Oracle Database and relational foundations](./chapters/01-oracle-database-and-relational-foundations.md)
02. [Connect with SQL Developer and SQLcl](./chapters/02-connect-with-sql-developer-and-sqlcl.md)
03. [Create schemas, tables, and Oracle data types](./chapters/03-schemas-tables-and-oracle-data-types.md)
04. [Select rows and shape results](./chapters/04-select-rows-and-shape-results.md)
05. [Filter, sort, and limit results](./chapters/05-filter-sort-and-limit-results.md)
06. [SQL functions, NULL, and conversions](./chapters/06-sql-functions-null-and-conversions.md)
07. [Joins and related tables](./chapters/07-joins-and-related-tables.md)
08. [Aggregates and grouping](./chapters/08-aggregates-and-grouping.md)
09. [Subqueries, CTEs, and set operators](./chapters/09-subqueries-ctes-and-set-operators.md)
10. [Insert, update, delete, and transactions](./chapters/10-data-changes-and-transactions.md)
11. [Keys, constraints, identity, and sequences](./chapters/11-keys-constraints-identity-and-sequences.md)
12. [Views, indexes, and execution plans](./chapters/12-views-indexes-and-execution-plans.md)
13. [PL/SQL blocks, variables, and control flow](./chapters/13-plsql-blocks-variables-and-control-flow.md)
14. [PL/SQL cursors, routines, packages, and errors](./chapters/14-plsql-routines-cursors-and-errors.md)
15. [Users, privileges, backup, and recovery](./chapters/15-security-backup-and-recovery.md)
16. [Use Oracle Database from JavaScript](./chapters/16-oracle-database-with-javascript.md)

98. [All code samples](./chapters/98-all-code-samples.md)
99. [Complete questions and answers](./chapters/99-complete-q-and-a.md)

## Official references

- [Oracle Database documentation](https://docs.oracle.com/en/database/)
- [Oracle SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/)
- [Oracle PL/SQL Language Reference](https://docs.oracle.com/en/database/oracle/oracle-database/23/lnpls/)
- [node-oracledb documentation](https://oracle.github.io/node-oracledb/)
## Links

- Portfolio: https://www.ashishranjan.net
- GitHub: https://github.com/a2rp
- CodePen: https://codepen.io/ash1198
- LinkedIn: https://www.linkedin.com/in/aashishranjan
- Facebook: https://www.facebook.com/theash.ashish/
- YouTube: https://www.youtube.com/@ashishranjan-ashz?sub_confirmation=1
- Email: mailto:ash.ranjan09@gmail.com

## Support

- Support: https://a2rp-donation-page.netlify.app/
- Buy Me a Coffee: https://buymeacoffee.com/ashishranjan
- Patreon: https://www.patreon.com/ashishranjan
