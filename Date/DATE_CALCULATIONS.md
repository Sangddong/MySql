## DATE CALCULATIONS

### 1. How to get year, month, date and day from specific data
- To select each data, you can use **YEAR, MONTH, DAY** and **DAYNAME**
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
👉🏻 following table is result of the query.
|name|birth|
|--|--|
|Tom|1998-07-13|
|Jerry|1999-08-24|
> ``` mysql
> SELECT name,
>        YEAR(birth) AS 'birth year',
>        MONTH(birth) AS 'birth month'
>        DAY(birth) AS 'birth date'
>        DAYNAME(birth) AS 'birth day'
> FROM birthday;
> ```
👉🏻 Data will be returned like :
|name|birth year|birth month|birth date|birth day|
|--|--|--|--|--|
|Tom|1998|7|13|Monday|
|Jerry|1999|8|24|Tuesday|

- Day can be returned in numeric value by **WEEKDAY**
- Monday = 0, Tuesday = 1, ... Sunday = 6
> ``` mysql
> SELECT name, DAYNAME(birth) AS 'birth day', WEEKDAY(birth) AS weekday
> FROM birthday;
> ```
👉🏻 Data will be returned like :
|name|birth day|weekday|
|--|--|--|
|Tom|Monday|0|
|Jerry|Tuesday|1|

