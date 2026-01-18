# ENUM vs CREATE DOMAIN

### 1. CREATE DOMAIN
```sql
CREATE DOMAIN GENDER CHAR(1)
CHECK (VALUE IN ('M', 'F'));
```
- 표준 sql 기능
- 기존 타입 CHAR, VARCHAR, INT 등을 기반
- `CHECK`, `NOT NULL`, `DEFAULT`와 같은 제약조건을 포함
- 여러 테이블/컬럼에서 재사용 가능

### 2. ENUM
```sql
CREATE TYPE GENDER AS ENUM ('M', 'F');
```
- DBMS(MySQL, postgreSQL) 의존적 기능
- 값이 리스트로 고정됨
- 내부적으로는 정수 매핑이라 성능/저장 효율 좋음
- 값 추가/삭제 시 스키마 변경 비용 큼

### 3. ENUM vs CREATE DOMAIN
|구분|CREATE DOMAIN|ENUM|
|--|--|--|
|SQL 표준|⭕|❌|
|DBMS 이식성|⭕ 높음|❌ 낮음|
|재사용성|⭕ 매우 좋음|⭕ 타입 재사용 가능|
|제약 표현력|⭕ 자유로움|❌ 값 목록만|
|의미 표현|⭕ 명확|❌ 제한적|
|값 변경|⭕ 비교적 유연|❌ 까다로움|
