# 6. SQL functions, NULL, and conversions

[Back to notes index](../README.md)

| [Previous: Filter, sort, and limit results](./05-filter-sort-and-limit-results.md) | [Notes index](../README.md) | [Next: Joins and related tables](./07-joins-and-related-tables.md) |
| --- | --- | --- |

## Replace or test missing values

Oracle functions can make missing values explicit in a result. **NVL** returns a fallback when its first argument is NULL. **COALESCE** returns the first non-NULL expression from its arguments:

~~~sql
SELECT employee_id,
       NVL(commission_pct, 0) AS commission_rate,
       COALESCE(department_id, 0) AS department_for_display
FROM employees;
~~~

Use a fallback that makes sense for the meaning of the column. Returning zero can be misleading if zero means a real measured value.

**NULLIF(a, b)** returns NULL when the arguments are equal. **COALESCE** is useful when several alternative values are available. Keep the result data types compatible.

## Use CASE for conditional values

A searched **CASE** expression selects a result based on conditions:

~~~sql
SELECT employee_id,
       CASE
         WHEN salary >= 100000 THEN 'Senior range'
         WHEN salary >= 60000 THEN 'Mid range'
         ELSE 'Entry range'
       END AS salary_band
FROM employees;
~~~

Conditions are checked in order, so place more specific or higher-priority conditions first. A CASE expression returns a value; it does not update the source row.

## Format values for display

Use **TO_CHAR** when a date or number needs a text representation:

~~~sql
SELECT employee_id,
       TO_CHAR(hire_date, 'YYYY-MM-DD') AS hire_date_text,
       TO_CHAR(salary, 'FM9999990.00') AS salary_text
FROM employees;
~~~

A formatting mask controls the output shape. Number and date formatting can be affected by session language or numeric settings, so specify the format required by the output.

Keep dates and numbers in their native types for comparisons, sorting, and calculations. Convert them to text at the presentation boundary.

## Convert input with explicit formats

Use an explicit format model when converting text to a date:

~~~sql
SELECT TO_DATE('2024-01-15', 'YYYY-MM-DD') AS parsed_date
FROM dual;
~~~

Avoid relying on implicit conversion rules such as the current session date format. Different sessions can interpret the same string differently.

For date ranges, use typed values and a half-open interval when the column includes a time component:

~~~sql
SELECT employee_id, hire_date
FROM employees
WHERE hire_date >= DATE '2024-01-01'
  AND hire_date < DATE '2025-01-01';
~~~

This includes all times on the first date and excludes the start of the following year.

## Apply date and numeric functions

Date values can be shifted or truncated with functions such as **ADD_MONTHS** and **TRUNC**:

~~~sql
SELECT employee_id,
       ADD_MONTHS(hire_date, 6) AS six_month_mark,
       TRUNC(hire_date) AS hire_day
FROM employees;
~~~

Numeric functions such as **ROUND** and **ABS** transform numeric results:

~~~sql
SELECT ROUND(AVG(salary), 2) AS average_salary
FROM employees;
~~~

Use functions in a WHERE condition deliberately. Applying a function to an indexed column may change how an index can be used. When possible, compare the original column to a correctly calculated range.

## Practice

Show a readable salary band with CASE. Replace a NULL commission only for display, then format a hire date. Compare a date-column range using start-inclusive and end-exclusive values.

## Check what you learned

1. What value does NVL return when its first argument is NULL?
2. How does COALESCE choose its result?
3. Why can replacing NULL with zero misrepresent data?
4. How does a searched CASE expression choose a value?
5. What is TO_CHAR used for?
6. Why should an explicit format model be used with TO_DATE?
7. Why is a half-open date range useful for columns with time values?
8. How can a function on a filtered column affect an index?

## References

- [Single-row functions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Single-Row-Functions.html)
- [NVL](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/NVL.html)
- [COALESCE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/COALESCE.html)
- [TO_DATE](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/TO_DATE.html)
- [Datetime functions](https://docs.oracle.com/en/database/oracle/oracle-database/23/sqlrf/Datetime-Functions.html)