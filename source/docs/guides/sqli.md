# SQL injection (SQLi)

SQL injection has been at the top of the OWASP vulnerability listings for many years, the reason being that, if identified and exploited to the full extent, they produce catastrophic outcomes.

* SQL injection vulnerabilities occur due to a lack of input validation.
* To identify a SQL injection bug, enter special characters to generate an error or unexpected behaviour.
* There are three main types of SQL injection: in-band, inferential or blind, and out-band.
* You can use the Intruder and Comparer tools in Burp, to automate SQL injection identification.
* Testing for SQL injection manually isn’t scalable. Using tools like SQLMap and NoSQLMap, it is possible to automate SQL injection exploitation.

## Steps

1. Map any of the application’s endpoints that take in user input.
2. Insert test payloads into these locations to discover whether they’re vulnerable to SQL injections. If the endpoint isn’t vulnerable to classic SQL injections, try inferential techniques instead.
3. Once you’ve confirmed that the endpoint is vulnerable to SQL injections, use different SQL injection queries to leak information from the database.
4. Escalate the issue. Figure out what data you can leak from the endpoint and whether you can achieve an authentication bypass. Be careful not to execute any actions that would damage the integrity of the target’s database, such as deleting user data or modifying the structure of the database.
5. Draft up an report with an example payload that the security team can use to duplicate your results. Because SQL injections are quite technical to exploit most of the time, it’s a good idea to spend some time crafting an easy-to-understand proof of concept.

## Look for classic SQL injections

Classic SQL injections are the easiest to find and exploit. In classic SQL injections, the results of the SQL query are returned directly to the attacker in an HTTP response. There are two subtypes: UNION based and error based.

## Look for blind SQL injections

Also called inferential SQL injections, blind SQL injections are a little harder to detect and exploit. They happen when attackers cannot directly extract information from the database because the application does not return SQL data or descriptive error messages. In this case, attackers can infer information by sending SQL injection payloads to the server and observing its subsequent behaviour. Blind SQL injections have two subtypes as well: Boolean based and time based.

## Exfiltrate information by using SQL injections

Imagine that the web application you’re attacking does not use your input in a SQL query right away. Instead, it uses the input unsafely in a SQL query during a backend operation, so you have no way to retrieve the results of injection via an HTTP response, or infer the query’s results by observing server behaviour. Sometimes there’s even a time delay between when you submitted the payload and when the payload gets used in an unsafe query, so you won’t immediately be able to observe differences in the application's behaviour.

In this case, you will need to make the database store information somewhere when it does run the unsafe SQL query.

In MySQL, the `SELECT. . .INTO` statement tells the database to store the results of a query in an output file on the local machine.

    SELECT Password FROM Users WHERE Username='admin'
    INTO OUTFILE '/var/www/html/output.txt'

Then access the information by navigating to the `/output.txt` page on the target: `https://
example.com/output.txt`. This technique is also a good way to detect second-order SQL injections, since in second-order SQL injections, there is often a time delay between the malicious input and the SQL query being executed.

## Look for NoSQL injections

Databases do not always use SQL. NoSQL, or Not Only SQL, databases are those that do not use the SQL language. Unlike SQL databases, which store data in tables, NoSQL databases store data in other structures, such as key-value pairs and graphs. NoSQL query syntax is database-specific, and queries are often written in the programming language of the application. Modern NoSQL databases, such as MongoDB, Apache CouchDB, and Apache Cassandra, are also vulnerable to injection attacks. These vulnerabilities are becoming more common as NoSQL rises in popularity.

In MongoDB syntax, `Users.find()` returns users that meet a certain criteria:

    Users.find({username: 'vickie', password: 'password123'});

If the application uses this functionality to log in users and populates the database query directly with user input, like this:

    Users.find({username: $username, password: $password});

attackers can submit the password `{$ne: ""}` to log in as anyone. If the attacker submits:

    Users.find({username: 'admin', password: {$ne: ""}});

In MongoDB, `$ne` selects objects whose value is not equal to the specified value, and the query would return users whose username is `admin` and password is not equal to an empty string.

Injecting into MongoDB queries can also allow attackers to execute arbitrary JavaScript code on the server. In MongoDB, the `$where`, `mapReduce`, `$accumulator`, and `$function` operations allow developers to run arbitrary JavaScript.

The process of looking for NoSQL injections is similar to detecting SQL injections. You can insert special characters such as quotes (`' "`), semi-colons (`;`), and backslashes (`\`), as well as parentheses (`()`), brackets(`[]`), and braces (`{}`) into user-input fields and look for errors or other anomalies.

You can also automate the hunting process by using the tool [NoSQLMap](https://github.com/codingo/NoSQLMap/).

## Escalation

Attackers most often use SQL injections to extract information from the database. Successfully collecting data from a SQL injection is a technical task that can sometimes be complicated.

Another way to escalate SQL injections is to attempt to gain a web shell on the server.

## Mitigations

Most instances of SQL injection can be prevented by using parameterized queries (prepared statements) instead of string concatenation within the query. 

Parameterized queries can be used for any situation where untrusted input appears as data within the query, including the `WHERE` clause and values in an `INSERT` or `UPDATE` statement. They can not be used to handle untrusted input in other parts of the query, such as table or column names, or the `ORDER BY` clause. Application functionality that places untrusted data into those parts of the query will need to take a different approach, such as white-listing permitted input values, or using different logic to deliver the required behaviour.

For a parameterized query to be effective in preventing SQL injection, the string that is used in the query must always be a hard-coded constant, and must never contain any variable data from any origin. Do not be tempted to decide case-by-case whether an item of data is trusted, and continue using string concatenation within the query for cases that are considered safe. It is all too easy to make mistakes about the possible origin of data, or for changes in other code to violate assumptions about what data is tainted. 

## Resources

* [Portswigger: SQL injection](https://portswigger.net/web-security/sql-injection)
* [OWASP: Testing for SQL Injection](https://owasp.org/www-project-web-security-testing-guide/stable/4-Web_Application_Security_Testing/07-Input_Validation_Testing/05-Testing_for_SQL_Injection)
* [HackTricks: SQL injection](https://book.hacktricks.xyz/pentesting-web/sql-injection)
* [Bug Bounty Bootcamp](https://nostarch.com/bug-bounty-bootcamp)
* [Bug Bounty Hunting Essentials](https://www.packtpub.com/product/bug-bounty-hunting-essentials/9781788626897)
* [Bug Bounty Hunting for Web Security](https://link.springer.com/book/10.1007/978-1-4842-5391-5)
