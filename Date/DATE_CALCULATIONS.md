## DATE CALCULATIONS

### 1. How to extract year, month, date, and day from a date
- You can use the **YEAR**, **MONTH**, **DAY**, and **DAYNAME** functions to extract specific parts of a date.
>``` mysql
> CREATE TABLE birthday (
>   name VARCHAR(100),
>   birth DATE
> );
>
> INSERT INTO birthday (name, birth)
> VALUES
> ('Tom', '1998-07-13'),
> ('Jerry', '1999-08-24');
>```
👉🏻 The following table shows the result of the query.
|name|birth|
|--|--|
|Tom|1998-07-13|
|Jerry|1999-08-24|
> ``` mysql
> SELECT name,
>        YEAR(birth) AS 'birth year',
>        MONTH(birth) AS 'birth month',
>        DAY(birth) AS 'birth date',
>        DAYNAME(birth) AS 'birth day'
> FROM birthday;
> ```
👉🏻 The following table shows the result of the query.
|name|birth year|birth month|birth date|birth day|
|--|--|--|--|--|
|Tom|1998|7|13|Monday|
|Jerry|1999|8|24|Tuesday|

- The **WEEKDAY** function returns the day of the week as a numeric value.
- Monday = 0, Tuesday = 1, ... Sunday = 6
> ``` mysql
> SELECT name, DAYNAME(birth) AS 'birth day', WEEKDAY(birth) AS weekday
> FROM birthday;
> ```
👉🏻 The following table shows the result of the query.
|name|birth day|weekday|
|--|--|--|
|Tom|Monday|0|
|Jerry|Tuesday|1|
### 2. How to calculate DATE Type data
#### (1) TIMESTAMPDIFF(sth, target1, target2)
- Used to calculate the time difference between two DATE values
> ``` mysql
> SET @TODAY = CURDATE();
> SELECT name,
>        TIMESTAMPDIFF(YEAR, birth, @TODAY) AS age,
>        TIMESTAMPDIFF(MONTH, birth, @TODAY) AS months,
>        TIMESTAMPDIFF(DAY, birth, @TODAY) AS days,
>        TIMESTAMPDIFF(WEEK, birth, @TODAY) AS week
> FROM birthday;
👉🏻 The following table shows the result of the query.
|name|age|months|days|week|
|--|--|--|--|--|
|Tom|26|321|9799|1399|
|Jerry|25|308|9392|1341|
#### (2) TIMESTAMPADD()
- Used to add a specific time interval to a DATE value
> ``` mysql
> SELECT name,
>        TIMESTAMPADD(YEAR, 1, birth) AS '1 year after',
>        TIMESTAMPADD(MONTH, 5, birth) AS '5 month after',
>        TIMESTAMPADD(DAY, 10, birth) AS '10 day after',
>        TIMESTAMPADD(WEEK, 15, birth) AS '15 week after'
> FROM birthday;
👉🏻 The following table shows the result of the query.
|name|1 year after|5 month after|10 day after|15 week after|
|--|--|--|--|--|
|Tom|1999-07-13|1998-12-13|1998-07-23|1999-10-26|
|Jerry|2000-08-24|2000-01-24|1999-09-03|1999-12-07|
#### (3) DATE_ADD and INTERVAL
- Another method to add a specific interval to a DATE value
> ``` mysql
> SELECT name,
>        DATE_ADD(birth, INTERVAL 1 YEAR) AS '1 year after',
>        DATE_ADD(birth, INTERVAL 5 MONTH) AS '5 month after',
>        DATE_ADD(birth, INTERVAL 10 DAY) AS '10 day after',
>        DATE_ADD(birth, INTERVAL 15 WEEK) AS '15 week after'
> FROM birthday;
👉🏻 The following table shows the result of the query.
|name|1 year after|5 month after|10 day after|15 week after|
|--|--|--|--|--|
|Tom|1999-07-13|1998-12-13|1998-07-23|1999-10-26|
|Jerry|2000-08-24|2000-01-24|1999-09-03|1999-12-07|
