## SOME WAYS TO MODIFY DATA

### 0. Updating data
- The most basic way to modify existing records.
- An UPDATE query keeps the primary key and any columns not specified in the query unchanged.
- The WHERE clause determines which rows will be updated.
  
> ```  mysql
> CREATE TABLE the_simpsons (
>   id INT(11) AUTO_INCREMENT PRIMARY KEY,
>   first_name VARCHAR(100) NOT NULL,
>   last_name VARCHAR(100) NOT NULL,
>   sex ENUM ('F', 'M') DEFAULT 'F'
> );
>
> INSERT INTO the_simpsons (first_name, last_name, sex)
> VALUES
> ('Homer', 'Simpsons', 'M'),
> ('Marge', 'Bouvier', 'F'),
> ('Bart', 'Simpsons', 'M'),
> ('Lisa', 'Simpsons', 'F'),
> ('Maggy', 'Simpsons', 'F');
> ```
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Homer|Simpson|M|
|2|Marge|Bouvier|F|
|3|Bart|Simpson|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|

### 1. UPDATE
- The most basic way to modify existing records.
- An UPDATE query keeps the primary key and any columns not specified in the query unchanged.
- The WHERE clause determines which rows will be updated.
> ``` mysql
> UPDATE the_simpsons
> SET last_name = 'Houten'
> WHERE sex = 'M';
👉🏻 The following table shows the result of the query.
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Homer|Houten|M|
|2|Marge|Bouvier|F|
|3|Bart|Houten|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|

- Only rows where sex = 'M' were updated.
- Primary keys and other unchanged columns remained the same.

### 2. REPLACE INTO
- **REPLACE INTO** updates existing rows when the PRIMARY KEY (or a UNIQUE key) matches a row in the table.
- If there is no matching row, it inserts a new one.
- Unlike INSERT, which only adds new rows, REPLACE deletes the matching row and then inserts the new one.
- This means existing data is completely replaced, not just updated.
> ``` mysql
> REPLACE INTO the_simpsons
> VALUES (1, 'Abe', 'Simpson');
👉🏻 The following table shows the result of the query.
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Abe|Simpson|M|
|2|Marge|Bouvier|F|
|3|Bart|Houten|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|

- Only the row with `id = 1` was replaced.
> ``` mysql
> REPLACE INTO the_simpsons
> VALUES (50, 'Homer', 'Simpson', 'M');
👉🏻 The following table shows the result of the query.
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Abe|Simpson|M|
|2|Marge|Bouvier|F|
|3|Bart|Houten|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|
|50|Homer|Simpson|M|

- A new row was inserted because no existing row matched `id = 50`.

### 3. ON DUPLICATE KEY UPDATE
- **ON DUPLICATE KEY UPDATE** updates an existing row when the PRIMARY KEY or a UNIQUE key matches a row in the table.
- If no matching row is found, a new one is inserted.
- Unlike **REPLACE INTO**, which deletes the matched row and inserts a new one, **ON DUPLICATE KEY UPDATE** simply updates the existing row without deleting it.
> ``` mysql
> INSERT INTO the_simpsons (id, first_name, last_name, sex)
> VALUES (1, 'Mona', 'Simpson', 'F')
> ON DUPLICATE KEY UPDATE
> first_name = VALUES(first_name),
> last_name = VALUES(last_name),
> sex = VALUES(sex);
👉🏻 The following table shows the result of the query.
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Mona|Simpson|F|
|2|Marge|Bouvier|F|
|3|Bart|Houten|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|
|50|Homer|Simpson|M|

- The row with `id = 1` was updated
> ``` mysql
> INSERT INTO the_simpsons (id, first_name, last_name, sex)
> VALUES (100, 'Abe', 'Simpson', 'F')
> ON DUPLICATE KEY UPDATE
> first_name = VALUES(first_name),
> last_name = VALUES(last_name),
> sex = VALUES(sex);
👉🏻 The following table shows the result of the query.
|id|first_name|last_name|sex|
|--|--|--|--|
|1|Mona|Simpson|F|
|2|Marge|Bouvier|F|
|3|Bart|Houten|M|
|4|Lisa|Simpson|F|
|5|Maggy|Simpson|F|
|50|Homer|Simpson|F|
|100|Homer|Simpson|M|

- A new row was inserted because no existing row matched `id = 100`.
