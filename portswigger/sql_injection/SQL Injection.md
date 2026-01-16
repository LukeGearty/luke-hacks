# SQL Injection

These are notes and screenshots from Portswigger's web-security modules on SQL injections. 

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

# LAB - SQL Injection Vulnerability in WHERE clause allowing retrieval of hidden data

<img width="1307" height="627" alt="sql_injection_where_clause-pt1" src="https://github.com/user-attachments/assets/7e009033-d588-4952-bed2-1627df8f4402" />

<img width="906" height="748" alt="sql_injection_where_clause_pt2" src="https://github.com/user-attachments/assets/9d6b8e43-57bb-4f21-93be-7b06e695f72f" />

<img width="908" height="389" alt="sql_injection_where_clause_pt3" src="https://github.com/user-attachments/assets/9a6874ec-9445-426d-8a8c-d84ad89c3a3c" />

<img width="1305" height="771" alt="sql_injection_where_clause_pt4" src="https://github.com/user-attachments/assets/be85f25c-6371-4eca-8c35-9f97646baaa2" />


# Subverting Application Logic

When you log in to an application with a username and password, the SQL query can look like this:

```
SELECT * FROM users WHERE username = 'username' AND password = 'password'
```

An attacker can use a SQL comment after username to remove the password check.


# LAB - SQL Injection vulnerability allowing login bypass

<img width="1313" height="631" alt="sql_injection_login_bypass-pt1" src="https://github.com/user-attachments/assets/5c8d32a8-5dcc-45e2-9ddd-6bfcfab0ac7a" />

<img width="1241" height="882" alt="sql_injection_login_bypass-pt2" src="https://github.com/user-attachments/assets/cb7e2e64-5a35-4f4f-ac43-a39d0be5a65e" />

<img width="914" height="467" alt="sql_injection_login_bypass-pt3" src="https://github.com/user-attachments/assets/9549764c-c47f-424e-8f91-78770368ff82" />

<img width="1308" height="625" alt="sql_injection_login_bypass-pt4" src="https://github.com/user-attachments/assets/0e1d7c18-3004-4866-bc12-adc0b90c7b7b" />


# Retrieving data from other database tables
When an application responds with the results of a SQL query, an attacker can use a SQL injection to retrieve data from other tables. 

# SQL Injection UNION attacks
UNION keyword enables you to execute one or more additional SELECT queries and append the results to the original query.
```
`SELECT a, b FROM table1 UNION SELECT c, d FROM table2`
```

Two requirements:
- The individual queries must return the same number of columns
- The data types in each column must be compatible between the individual queries.
When performing a SQL injection UNION attack, you need to know how many columns are being returned from the original query, and which columns returned from the original query are of a suitable data type to hold the results from the injected query.

## Determining the number of columns

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


# LAB - SQL injection UNION attack, determining the number of columns returned by the query

<img width="1329" height="842" alt="sqli_union_determine_num_columns_pt1" src="https://github.com/user-attachments/assets/cbca9758-4bf2-4ed6-bfdc-3d4379dbeaf6" />

<img width="1250" height="754" alt="sqli_union_determine_num_columns_pt2" src="https://github.com/user-attachments/assets/ed4376ed-122a-4439-bc6f-af60e035bafd" />

<img width="912" height="758" alt="sqli_union_determine_num_columns_pt3" src="https://github.com/user-attachments/assets/ee782ba8-fac4-4995-834b-d0ee87e13bfc" />

<img width="1326" height="630" alt="sqli_union_determine_num_columns_pt4" src="https://github.com/user-attachments/assets/97ddb42a-9dbb-4b18-aa72-7875810bea95" />


# Database-specific syntax

**Oracle**: every select query must use the FROM keyword and specify a valid table. In Oracle, there is a built-in table called Dual.
```
' UNION SELECT NULL FROM DUAL--
```

# Finding Columns with a useful data type

The data you would usually want from a query is usually in string form (username, password, etc). So one of the columns needs to be compatible with string data.
```
' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 
' UNION SELECT NULL,NULL,NULL,'a'--`
```

If the column data type is not compatible, the injected query will cause a database error.

# LAB - SQL injection UNION attack, finding a column containing text

<img width="1327" height="770" alt="sqli_union_attack_finding_col_text_pt1" src="https://github.com/user-attachments/assets/5cd52c78-2e09-47ad-9d7b-6be16608842a" />

<img width="913" height="808" alt="sqli_union_attack_finding_col_text_pt2" src="https://github.com/user-attachments/assets/3de8195b-0749-4bcc-9e3f-b9110984e46c" />

<img width="914" height="720" alt="sqli_union_attack_finding_col_text_pt3" src="https://github.com/user-attachments/assets/4f904ba2-a771-4e7c-a76c-218e8aa10c85" />

<img width="901" height="711" alt="sqli_union_attack_finding_col_text_pt4" src="https://github.com/user-attachments/assets/4173f113-bb99-4811-8a32-ce9024dd4346" />

<img width="909" height="806" alt="sqli_union_attack_finding_col_text_pt5" src="https://github.com/user-attachments/assets/c7619a03-232d-4192-ae5c-47790125ea78" />

<img width="1329" height="636" alt="sqli_union_attack_finding_col_text_pt6" src="https://github.com/user-attachments/assets/1c94d724-9e87-4ae3-936c-dfe4b40c4e8f" />


# LAB - SQL injection UNION attack, retrieving data from other tables

<img width="1113" height="772" alt="sqli_union_retrieving_data_from_other_tables-pt1" src="https://github.com/user-attachments/assets/5be9b353-8b9c-43a2-8789-68c5d0e37128" />

<img width="1241" height="756" alt="sqli_union_retrieving_data_from_other_tables-pt2" src="https://github.com/user-attachments/assets/93e041fd-bd6b-4aeb-9269-2b49afea7bdf" />

<img width="1253" height="833" alt="sqli_union_retrieving_data_from_other_tables-pt3" src="https://github.com/user-attachments/assets/d5c78b2d-9f02-4b77-9a93-caae1afb803c" />

<img width="1247" height="762" alt="sqli_union_retrieving_data_from_other_tables-pt4" src="https://github.com/user-attachments/assets/1f8c2d06-a941-436e-bf25-b3c73bc25e8b" />

<img width="1113" height="774" alt="sqli_union_retrieving_data_from_other_tables-pt5" src="https://github.com/user-attachments/assets/5bb8c42d-21ca-4bb3-9e91-44410017d3c4" />


# LAB - SQL injection UNION attack, retrieving multiple values in a single column

<img width="1115" height="770" alt="sqli_union_retrieving_multiple_values_single_column_pt1" src="https://github.com/user-attachments/assets/eb9f40c3-2a4b-4d0c-a3cf-c24d3d710ab3" />

<img width="1584" height="752" alt="sqli_union_retrieving_multiple_values_single_column_pt2" src="https://github.com/user-attachments/assets/c8d8895f-74e7-4202-81b0-12ecdb0375ba" />

<img width="1580" height="710" alt="sqli_union_retrieving_multiple_values_single_column_pt3" src="https://github.com/user-attachments/assets/d39a1d1e-ea65-41e8-a09d-0af990fab5a0" />

<img width="1578" height="744" alt="sqli_union_retrieving_multiple_values_single_column_pt4" src="https://github.com/user-attachments/assets/50e7e8c2-16c2-4875-9718-a122004dd171" />

<img width="1585" height="827" alt="sqli_union_retrieving_multiple_values_single_column_pt5" src="https://github.com/user-attachments/assets/71984646-606c-437a-9e25-6e7de9ed3411" />

<img width="1128" height="783" alt="sqli_union_retrieving_multiple_values_single_column_pt6" src="https://github.com/user-attachments/assets/ea85439c-936a-468d-9d98-c4d77bba9473" />


# Retrieving Multiple Values within a single column
In some cases the query may only return a single column. You can retrieve multiple values together within the single column by concatenating the values together.

On Oracle:
```
' UNION SELECT username || '~' || password FROM users--
```

# Examining the database in SQL injection attacks 

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


# LAB - SQL injection attack, querying the database type and version on Oracle
<img width="1330" height="772" alt="sql_injection_attack_querying_database_type_on_oracle_pt1" src="https://github.com/user-attachments/assets/7c4dd213-4091-45f0-9b6e-20a22ed7bb42" />

<img width="1250" height="759" alt="sql_injection_attack_querying_database_type_on_oracle_pt2" src="https://github.com/user-attachments/assets/29aa885a-ff29-49df-87d6-6a8bd11cb031" />

<img width="904" height="716" alt="sql_injection_attack_querying_database_type_on_oracle_pt3" src="https://github.com/user-attachments/assets/4f100425-e931-41fa-8fea-c889bcaf75fe" />


<img width="908" height="712" alt="sql_injection_attack_querying_database_type_on_oracle_pt4" src="https://github.com/user-attachments/assets/891aad0e-edba-4565-9968-fbca6ab97f91" />

<img width="909" height="712" alt="sql_injection_attack_querying_database_type_on_oracle_pt5" src="https://github.com/user-attachments/assets/1b0dff18-06ff-45d4-87c3-6a727b88fd2c" />

<img width="915" height="755" alt="sql_injection_attack_querying_database_type_on_oracle_pt6" src="https://github.com/user-attachments/assets/d7952a5a-8248-4a3a-a7b6-26c26a90acc5" />

# LAB -SQL injection attack, querying the database type and version on MySQL and Microsoft

<img width="1119" height="776" alt="sqli_query_database_mysql_pt1" src="https://github.com/user-attachments/assets/ad57ecd8-4621-48aa-b703-5f65ffd90fcb" />

<img width="1582" height="790" alt="sqli_query_database_mysql_pt2" src="https://github.com/user-attachments/assets/2506661f-bb22-49b8-92d3-dbd38d910bda" />

<img width="1580" height="747" alt="sqli_query_database_mysql_pt3" src="https://github.com/user-attachments/assets/9901b8fd-aadd-47c5-93b0-149faa3a5677" />

<img width="1116" height="774" alt="sqli_query_database_mysql_pt4" src="https://github.com/user-attachments/assets/4bb3b6d4-cd70-4439-adff-cabdb1d321be" />


# Listing the Contents of the Database

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


# LAB - SQL injection attack, listing the database contents on non-Oracle databases

<img width="1112" height="772" alt="sqli_attack_database_contents_non_oracle_pt1" src="https://github.com/user-attachments/assets/b7ef0f3f-3956-4575-a8d5-40a8972beb4b" />

<img width="1247" height="858" alt="sqli_attack_database_contents_non_oracle_pt2" src="https://github.com/user-attachments/assets/df8be0b0-882c-4104-b4cd-c0b3173ff206" />

<img width="1253" height="886" alt="sqli_attack_database_contents_non_oracle_pt3" src="https://github.com/user-attachments/assets/7a308ae5-e764-496d-ad31-046fdfcdc02e" />

<img width="1244" height="764" alt="sqli_attack_database_contents_non_oracle_pt4" src="https://github.com/user-attachments/assets/658f43f1-2054-4dea-9789-8322092150b2" />

<img width="1247" height="762" alt="sqli_attack_database_contents_non_oracle_pt5" src="https://github.com/user-attachments/assets/9e8446ed-347a-4d20-9f87-a0e59e38facd" />

<img width="1249" height="857" alt="sqli_attack_database_contents_non_oracle_pt6" src="https://github.com/user-attachments/assets/2b9963ac-f2e4-4816-b3b5-ab01918b00a4" />

<img width="1248" height="861" alt="sqli_attack_database_contents_non_oracle_pt7" src="https://github.com/user-attachments/assets/6b859cce-8561-4bcf-b69a-6b0c48b12f78" />

<img width="1115" height="771" alt="sqli_attack_database_contents_non_oracle_pt8" src="https://github.com/user-attachments/assets/3b7b39de-a2e7-4126-bcd8-6da9cd394839" />


# Blind SQL Injection
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

# Lab - Blind SQL injection with conditional responses

<img width="1121" height="775" alt="blind_sqli_conditional_responses_pt1" src="https://github.com/user-attachments/assets/4c012588-32d4-4be9-9520-e429bb19c07c" />

<img width="1246" height="750" alt="blind_sqli_conditional_responses_pt2" src="https://github.com/user-attachments/assets/5f1f3261-ba5f-45a5-bea5-201befe973ea" />

<img width="1108" height="233" alt="blind_sqli_conditional_responses_pt3" src="https://github.com/user-attachments/assets/d91787c4-8da8-4002-b0b6-12c2097ee511" />

<img width="1251" height="844" alt="blind_sqli_conditional_responses_pt4" src="https://github.com/user-attachments/assets/46665a46-2fa6-4fd4-b6bc-86a2ec046e65" />

<img width="1249" height="861" alt="blind_sqli_conditional_responses_pt5" src="https://github.com/user-attachments/assets/da9eca70-1a52-4d44-a695-717ed453d29b" />

<img width="1480" height="835" alt="blind_sqli_conditional_responses_pt6" src="https://github.com/user-attachments/assets/07d65cea-7954-4650-8ed5-fbb500ec5f30" />

<img width="1114" height="772" alt="blind_sqli_conditional_responses_pt7" src="https://github.com/user-attachments/assets/b8375e26-d155-4d05-a3c5-b6c39832c2ea" />

# Error Based SQL Injection

SQL Injections based on errors - inducing a specific response based on the boolean expression. Induce the application to return a different response even if the query doesn't return data - cause a database error only if condition is true.

When applications carry out SQL queries but their behavior doesn't change, it can be possible to induce the app to return a different response based on whether a SQL error occurs. If the page behaves differently when the SQL error occurs, then you can infer whether a condition was true or false.

Example:

```
xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a
xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
```

First condition roughly translates to:
`If (1=2) return 1/0 else return 'a'`
Second:
`If (1=1) return 1/0 else return 'a'`

First condition returns 'a', second returns 1/0, which causes a divide by 0 error. 

# Lab - Blind SQL injection with conditional errors

<img width="1116" height="883" alt="blind_sqli_conditional_errors_pt1" src="https://github.com/user-attachments/assets/b01bbd69-0ecf-48b7-8551-53f570ef5529" />

<img width="1249" height="687" alt="blind_sqli_conditional_errors_pt2" src="https://github.com/user-attachments/assets/d28ea0fd-5bcb-4780-aece-c58952154d30" />


<img width="1267" height="783" alt="blind_sqli_conditional_errors_pt3" src="https://github.com/user-attachments/assets/8f8b7b79-df6f-42c3-85e1-c060c4d31cfd" />

<img width="1249" height="365" alt="blind_sqli_conditional_errors_pt4" src="https://github.com/user-attachments/assets/74edabbc-098d-4843-bc0f-0aea4bf32d00" />

<img width="1265" height="641" alt="blind_sqli_conditional_errors_pt5" src="https://github.com/user-attachments/assets/6eaa65bf-a07c-4035-8d92-770e18c45b63" />

<img width="1072" height="361" alt="blind_sqli_conditional_errors_pt6" src="https://github.com/user-attachments/assets/f091836a-9156-4410-8b86-8a645c3a9741" />

<img width="762" height="636" alt="blind_sqli_conditional_errors_pt7" src="https://github.com/user-attachments/assets/e1167b91-c0b3-4e9a-b361-2249d41bbb40" />

<img width="1481" height="511" alt="blind_sqli_conditional_errors_pt8" src="https://github.com/user-attachments/assets/8ee9e006-3674-43ce-a191-3008923e0841" />

<img width="844" height="769" alt="blind_sqli_conditional_errors_pt9" src="https://github.com/user-attachments/assets/c06d5ee3-2893-4e2b-9a23-ac7c0455c577" />


# LAB - SQL injection with filter bypass via XML encoding

<img width="1330" height="838" alt="sql_injection_filter_bypass_xml_encoding-pt1" src="https://github.com/user-attachments/assets/7603bcf2-dec8-44f3-a963-3b3c59e29eff" />

<img width="909" height="465" alt="sql_injection_filter_bypass_xml_encoding-pt2" src="https://github.com/user-attachments/assets/c71886a0-890c-4670-8883-281b0847fe25" />

<img width="903" height="714" alt="sql_injection_filter_bypass_xml_encoding-pt3" src="https://github.com/user-attachments/assets/c168b245-7ecb-4eaa-9842-79b436f7bca9" />

<img width="907" height="717" alt="sql_injection_filter_bypass_xml_encoding-pt4" src="https://github.com/user-attachments/assets/ddf50c4f-d8db-42b9-8d06-32691d4edbae" />

<img width="911" height="716" alt="sql_injection_filter_bypass_xml_encoding-pt5" src="https://github.com/user-attachments/assets/cc244fa4-71c0-49f9-b2c2-2cf2b5412c54" />

<img width="912" height="715" alt="sql_injection_filter_bypass_xml_encoding-pt6" src="https://github.com/user-attachments/assets/48da4022-6840-47e6-9b23-61993e3c5842" />

<img width="1330" height="766" alt="sql_injection_filter_bypass_xml_encoding-pt7" src="https://github.com/user-attachments/assets/6e9c276a-1c32-4370-96fa-a1190f9f4da0" />

