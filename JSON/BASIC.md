# JSON_BASIC

example data

```sql
SELECT * FROM json;
```

->

| data                                                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {"name":"sanghee","age":27,"salaryList":[{"month":"JAN","salary":1000},{"month":"FEB","salary":2000},{"month":"MAR","salary":3000},{"month":"APR","salary":4000}]} |
| sanghee park                                                                                                                                                        |
| 1                                                                                                                                                                   |

---

### 1. JSON_VALID()

* Returns whether the value is type of JSON

```sql
SELECT JSON_VALID(data)
FROM json;
```

->

| JSON_VALID(data) |
| ----------------- |
| 1                 |
| 0                 |
| 1                 |

---

### 2. JSON_PRETTY()

* Returns organized form of JSON

```sql
SELECT JSON_PRETTY(data)
FROM json
WHERE JSON_VALID(data);
```

->

| JSON_PRETTY(data) |
| ------------------ |
| ```json         |
| {                  |
| "age": 27,         |
| "name": "sanghee", |
| "salaryList": [   |

```
{ "month": "JAN", "salary": 1000 },
{ "month": "FEB", "salary": 2000 },
{ "month": "MAR", "salary": 3000 },
{ "month": "APR", "salary": 4000 }
```

]
}

````|

---

### 3. JSON_OBJECT(key, val, key, val, ...)
- Makes JSON data
```sql
SELECT JSON_OBJECT('name', 'sanghee', 'age', 27, 'job', 'developer') AS obj;
````

->

| obj                                                |
| -------------------------------------------------- |
| {"name": "sanghee", "age": 27, "job": "developer"} |

---

### 4. JSON_VALUE(col, '$.target_key_1[index].target_key_2')

* Returns specific data from the data type of JSON

```sql
SELECT JSON_VALUE(data, '$.salaryList[0].salary') AS first_month
FROM json;
```

->

| first_month |
| ------------ |
| 1000         |

```sql
SELECT JSON_VALUE(data, '$.name') AS user_name
FROM json;
```

->

| user_name |
| ---------- |
| sanghee    |

---

### 5. JSON_SEARCH(col, one_or_all, target_value)

* Searches target_value's path of the data type of JSON
* one_or_all

  * one : returns one path where the target_value was found
  * all : returns all path where the target_value was found

```sql
SELECT JSON_SEARCH(data, 'one', 'FEB') AS path_found
FROM json
WHERE JSON_VALID(data);
```

->

| path_found             |
| ----------------------- |
| $.salaryList[1].month |

```sql
SELECT JSON_SEARCH(data, 'all', 3000) AS path_found
FROM json
WHERE JSON_VALID(data);
```

->

| path_found              |
| ------------------------ |
| $.salaryList[2].salary |

---

### 6. JSON_EXTRACT(col, path)

* Returns JSON fragment at the given path

```sql
SELECT JSON_EXTRACT(data, '$.salaryList[1]') AS feb_salary
FROM json
WHERE JSON_VALID(data);
```

->

| feb_salary                      |
| -------------------------------- |
| {"month": "FEB", "salary": 2000} |

---

### 7. JSON_SET(col, path, val)

* Updates or inserts data into JSON

```sql
SELECT JSON_SET(data, '$.age', 28) AS updated_data
FROM json
WHERE JSON_VALID(data);
```

->

| updated_data                                                                                                                                                       |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| {"name":"sanghee","age":28,"salaryList":[{"month":"JAN","salary":1000},{"month":"FEB","salary":2000},{"month":"MAR","salary":3000},{"month":"APR","salary":4000}]} |

---

### 8. JSON_ARRAY(elements...)

* Creates a JSON array

```sql
SELECT JSON_ARRAY('apple', 'banana', 'cherry') AS fruit_list;
```

->

| fruit_list                  |
| ---------------------------- |
| ["apple","banana","cherry"] |
