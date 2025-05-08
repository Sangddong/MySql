## Enum vs. SET

### 1. ENUM
#### (1) What is **ENUM**
- A data type that limits a column to a set of permitted (or predefined) string values.

#### (2) **Advatages** of using ENUM
- Compact data storage: Every specified string value is automatically encoded into a numeric value, allowing for compact storage
> Ex) 
    CREATE TABLE color (
        gender ENUM('red', 'blue', 'yellow')
    );
> 👉🏻 internally, data will be stored as 
> : red = 0, blue = 1, yellow = 2

#### (3) Creating and using of ENUM column
- create table
    CREATE TABLE cake (
        color ENUM('pink', 'yellow'),
        size ENUM('small', 'medium', 'large')
    );
- insert value
    INSERT INTO cake (color, size) VALUES 
    ('pink', 'small'),
    ('pink','medium'),
    ('yellow', 'medium'),
    ('yellow', 'large');
- update value
    UPDATE cake
    SET size = 'large'
    WHERE color = 'pink' AND size = 'small';
- select data
    SELECT *
    WHERE color = 'yellow';

#### (4) Numeric values of ENUM
- Note that ENUM value is set of string values.
- If you try to use numeric value at ENUM type column, MySql recognizes it as **index**.
> Ex)
    CREATE TABLE numbers (
        number ENUM('1', '2', '3')
    )

    INSERT INTO numbers (number) VALUES
    (1),    -- stored as '2'
    ('2')   -- stored as '2'
    (2);    -- stored as '3'


