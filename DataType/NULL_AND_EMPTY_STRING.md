## NULL AND EMPTY STRING
### 1. NULL in MySql
- means no value (undefied, unknown)
- result is also NULL when NULL used at calculation
- can't compared by `=`
- must use `is null`, `is not null` at comparison

### 2. '' in Mysql
- string that has length = 0
- it has value not content
- can only be used string type

### 3. NULL and '' in MySql
- treats them as completely different values

### 4. NULL in Oracle
- means no value (undefied, unknown)
- result is also NULL when NULL used at calculation
- can't compared by `=`
- must use `is null`, `is not null` at comparison

### 5. '' in Oracle
- treats as NULL

### 6. NULL and '' in Oracle
- '' == NULL 성립

||MySql|Oracle|
|--|--|--|
|`NULL`|값 없음|값 없음|
|`''`|값 있음|값 없음|
|`비교`|NULL != ''|NULL == ''|
