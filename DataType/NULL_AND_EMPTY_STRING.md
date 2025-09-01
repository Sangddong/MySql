## NULL AND EMPTY STRING
### 1. NULL in MySql
- 값이 존재하지 않음을 의미 (미지정, 알 수 없음)
- 연산에 사용 시, 결과값도 null
- = 연산자로는 비교할 수 없음
- 비교 시 is null, is not null 사용해야 함

### 2. '' in Mysql
- 길이가 0 인 문자열
- 값은 존재하되, 내용은 없음
- 문자열 타입에서만 가능

### 3. NULL and '' in MySql
- 서로 완전히 다른 값으로 취급

### 4. NULL in Oracle
- 값이 존재하지 않음을 의미 (미지정, 알 수 없음)
- 연산에 사용 시, 결과값도 null
- = 연산자로는 비교할 수 없음
- 비교 시 is null, is not null 사용해야 함

### 5. '' in Oracle
- NULL로 취급

### 6. NULL and '' in Oracle
- '' == NULL 성립

||MySql|Oracle|
|--|--|--|
|`NULL`|값 없음|값 없음|
|`''`|값 있음|값 없음|
|`비교`|NULL != ''|NULL == ''|
