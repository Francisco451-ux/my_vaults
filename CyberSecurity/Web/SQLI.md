## What is SQL?
```
select * from users;
```
```
select username,password from users;
```
```
select * from users LIMIT 1;
```
```
select * from users where username='admin';
```
```
select * from users where username != 'admin';
```
```
select * from users where username='admin' or username='jon';
```
```
select * from users where username='admin' and password='p4ssword';
```
```
select * from users where username like 'a%'; uernames com um a inicio
```
```
select * from users where username like '%n'; usernames com um n fim
```
```
select * from users where username like '%mi%';
```
```
SELECT name,address,city,postcode from customers UNION SELECT
company,address,city,postcode from suppliers;
insert into users (username,password) values ('bob','password123');
update users SET username='root',password='pass123' where username='admin';
delete from users where username='martin';
delete from users;
```

What is SQL Injection?

https://website.thm/blog?id=1

From the URL above, you can see that the `blog` entry been selected comes from the `id` parameter in the query string. The web application needs to **retrieve the article** from the **database** and may use an SQL statement that looks something like the following:
```
SELECT * from blog where id=1 and private=0 LIMIT 1;
```
```
https://website.thm/blog?id=2;--
```
```
SELECT * from blog where id=2;-- and private=0 LIMIT 1;
```
```
SELECT * from blog where id=2;--
```

## In-Band SQL Injection

Discovering an SQL Injection vulnerability on a website page and then being able to **extract** data from the **database** to the same page


## Error-Based SQL Injection

Easily obtaining **information** about the database **structure** as error messages from the database are **printed directly** to the **browser screen**. This can often be used to **enumerate** a whole database.

## Union-Based SQL Injection
This type of Injection utilises the SQL `UNION` operator alongside a `SELECT` statement to return **additional results** to the page. This method is the most common way of **extracting** large **amounts of data** via an SQL Injection vulnerability.



## How To exploit SQLI

The key to discovering error-based SQL Injection is to break the code's SQL query by **trying certain characters** until an error message is produced; these are most commonly single apostrophes (`'` ) or a quotation mark ( `"` ).

Try typing an apostrophe ( `'` ) after the id=1 and press enter. And you'll see this returns an **SQL error informing you of an error in your syntax**. The `fact` that you've **received this error message** confirms the existence of an SQL Injection vulnerability. We can now exploit this vulnerability and use the error messages to learn more about the database structure.

