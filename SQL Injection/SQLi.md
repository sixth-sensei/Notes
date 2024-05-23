## What is SQLi?
SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. In many cases, it allows to modify or delete this data, causing persistent changes to the application's content or behavior.

## Detecting SQLi Vulnerabilities
- Submiting the single quote `'` at the entry point of an application and look for errors or other anomalies
- Boolean conditions such as `OR 1=1` and `OR 1=2`, and look for differences in the application's responses.

## SQLi Examples
### Retrieving Hidden Data
Example:

Request URL `https://sqlitest.com/products?category=Gifts`

Corresponding SQL query `SELECT * FROM products WHERE category = 'Gifts' AND released = 1`

_NB:_ `released = 1` is to show products that have been released only

To inject this query and reveal hidden products, SQL comment indicator `--` or `#` is added to the position where released condition is being checked for. Doing this ignores all other commands after it

Injected SQL query `SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1`

Injected request URL `https://sqlitest.com/products?category=Gifts'--`

This can also be performed by adding `' OR 1=1--` in the same position to reveal products in other categories that might be present

### Subverting Application Logic
This is done to allow attackers bypass authentication and gain access to the system as an authenticated user using SQL comment indicator.

Example:

Login query `SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'`

To inject this query and authenticate without a password, simply supply `--` after the username field

Injected query `SELECT * FROM users WHERE username = 'wiener'--' AND password = 'bluecheese'`

### Retrieving data from other database tables
If an application responds with the results of a SQL query, SQL injection can be used to retrieve data from other tables within the database. This is done using `UNION` keyword to execute additional `SELECT` query i.e adding `' UNION SELECT field1, field2 from table1--` to an existing `SELECT` query