## Enum vs. SET

### 1. ENUM
#### (1) What is **ENUM**
- A data type that limits a column to a set of permitted string values.
- Each value is encoded into a numeric value, which can be used as an index.
- Each column entry can hold one of the predefined values or NULL.

#### (2) **Advatages** of using ENUM
- Compact data storage: Every specified string value is automatically encoded into a numeric value, allowing for compact storage
    
    > ``` mysql
    > CREATE TABLE color (
    >     gender ENUM('red', 'blue', 'yellow')
    > );
    > ```
    > 👉🏻  internally, data will be stored as :
    > red = 0, blue = 1, yellow = 2

#### (3) Creating and using of ENUM column
- create table
     
    > ```mysql
    > CREATE TABLE cake (
    >     color ENUM('pink', 'yellow'),
    >     size ENUM('small', 'medium', 'large')
    > );
    > ```
- insert value
     
    >```mysql
    > INSERT INTO cake (color, size) VALUES 
    > ('pink', 'small'),
    > ('pink','medium'),
    > ('yellow', 'medium'),
    > ('yellow', 'large');
    >```
- update value
     
    > ```mysql
    > UPDATE cake
    > SET size = 'large'
    > WHERE color = 'pink' AND size = 'small';
    > ```
- select data   
    > ```mysql
    > SELECT *
    > FROM cake
    > WHERE color = 'yellow';
    > ```

#### (4) Numeric values of ENUM
- Note that ENUM value is set of string values.
- If you try to use **numeric value** at ENUM type column, MySql recognizes it as **index**.
      
    > ```mysql
    > CREATE TABLE numbers (
    >     value ENUM('1', '2', '3')
    > )
    >
    > INSERT INTO numbers (value) VALUES
    > (1),    -- stored as '2'
    > ('2'),   -- stored as '2'
    > (2);    -- stored as '3'
    >```
   
### 2. SET
#### (1) What is **SET**
- A data type that limits a column to a set of permitted string values.
- Each column entry can hold **multiple** values from the predefined set, or NULL.

#### (2) **Advantages** of using SET
- Multiple selection: Each column entry can hold multiple values, making it suitable for checkbox-style inputs.
- Simple query support: Multiple values can be searched using the FIND_IN_SET() function or bitwise & operations.

#### (3) Creating and using of SET column
- create table
     
    > ```mysql
    > CREATE TABLE words (
    >     alphabet SET('a', 'b', 'c', 'd')
    > );
    > ```
    > 👉🏻 internally, values will have following binary values :
    > 'a' = 1, 'b' = 2, 'c' = 4, 'd' = 8
- insert value
     
    >```mysql
    > INSERT INTO words (alphabet) VALUES 
    > ('a', 'b', 'c'),    -- stored as 'a', 'b', 'c'
    > ('a', 'b', 'b'),    -- stored as 'a', 'b'
    > ('c', 'a'),    -- stored as 'a', 'c'
    > ('d', 'c', 'c');    -- stored as 'c', 'd'
    >```
- select data   
    > ```mysql
    > -- (1) and (2) return same result
    > -- (1)
    > SELECT *
    > FROM words
    > WHERE FIND_IN_SET('a', alphabet) > 0
    > OR FIND_IN_SET('b', alphabet) > 0
    > OR FIND_IN_SET('d', alphabet) > 0;
    > -- (2)
    > SELECT *
    > FROM words
    > WHERE alphabet LIKE '%a%' OR alphabet LIKE '%b%' OR alphabet LIKE '%d%';
    > -- (3)
    > SELECT *
    > FROM words
    > WHERE FIND_IN_SET(11, alphabet); -- 11 = 1 + 2 + 8 -> returns rows that include 'a' or 'b' or 'd'
    > ```
