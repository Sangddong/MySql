# JSON_BASIC
example data
``` sql
SELECT * FROM json
```
-> 
|data|
|-|
|{"name":"sanghee","age":27,"salaryList":[{"month":"JAN","salary":1000},{"month":"FEB","salary":2000},{"month":"MAR","salary":3000},{"month":"APR","salary":4000}]}|
|sanghee park|
|1|

### 1. JSON_VALID()
- Returns wheteher the value is type of JSON
``` sql
SELECT JSON_VALID(data)
FROM json;
```
->
|JSON_VALID(data)|
|-|
|1|
|0|
|1|

### 2. JSON_PRETTY()
- Returns organized form of JSON
``` sql
SELECT JSON_PRETTY(data)
FROM json
WHERE JSON_VALID(data)
```
->
|JSON_VALID(data)|
|-------------------------|
| ```json
{
  "age": 27,
  "name": "sanghee",
  "salaryList": [
    { "month": "JAN", "salary": 1000 },
    { "month": "FEB", "salary": 2000 },
    { "month": "MAR", "salary": 3000 },
    { "month": "APR", "salary": 4000 }
  ]
}
``` |
|1|

### 3. JSON_OBJECT(key, val, key, val, ...)
- Makes JSON data

### 4. JSON_VALUE(col, '$.target_key_1[index].target_key_2')
- Returns specific data from the data type of JSON

### 5. JSON_SEARCH(col, one_or_all, target_value)
- Searches target_value's path of the data type of JSON
- one_or_all
  - one : returns one path where the target_value was found
  - all : returns all path where the target_value was found
