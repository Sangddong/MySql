## NULL AND EMPTY STRING
### 1. `NULL` in MySql
- Means no value (`undefined`, `unknown`)
- Any calculation with `NULL` results in `NULL`
- Cannot be compared using `=`
- Must use `IS NULL` or `IS NOT NULL` for comparison

### 2. `''` in Mysql
- A string with length = 0
- Has a value, but no content
- Can only be used with string types

### 3. `NULL` and `''` in MySql
- Treated as completely different values

### 4. `NULL` in Oracle
- Means no value (`undefined`, `unknown`)
- Any calculation with `NULL` results in `NULL`
- Cannot be compared using `=`
- Must use `IS NULL` or `IS NOT NULL` for comparison

### 5. `''` in Oracle
- Treated as `NULL`

### 6. NULL and '' in Oracle
- `''` is considered equal to `NULL`

||MySql|Oracle|
|--|--|--|
|`NULL`|no value|no value|
|`''`|Value exists (empty string)|Treated as `NULL`|
|`비교`|`NULL != ''`|`NULL == ''`|
