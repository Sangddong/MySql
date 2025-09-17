# 대용량 테이블에서 효율적으로 검색하는 방법
```sql
CREATE TABLE error_log_table (
  error_seq INT PRIMARY KEY AUTO_INCREMENT,
  api VARCHAR(100) NOT NULL,
  payload TEXT NOT NULL,
  reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
---
### 1. `WHERE`절 
- `WHERE`절은 옵티마이저가 조건을 재배치 후 실행 계획 결정
- **인덱스 컬럼을 포함**시켜 **풀스캔 방지**
Example)
``` sql
-- ❌ DON'T
SELECT *
FROM error_log_table
WHERE payload like '%example%';

-- ✅ DO
SELECT *
FROM error_log_table
WHERE error_seq >= 10000
      AND error_seq < 20000
      AND reg_date >= '2025-09-01'
      AND reg_date < '2025-10-01';
```
---
### 2. `OFFSET`보다 `SEEK`방식
- `OFFSET N LIMIT M` : DB가 앞의 N개 행을 건너뛰기 위해 스캔 -> 비용 = O(N + M) -> **매우 느림**
- `SEEK` 방식 : 키셋 페이징 방식
  - 마지막으로 본 행의 정렬 키 값을 기준으로 다음 페이지를 가져오는 방식
  - **일관된 응답 속도** 제공, **효율적**
<br>

Example)
``` sql
-- ❌ DON'T
SELECT *
FROM error_log_table
ORDER BY error_seq ASC
OFFSET 1000 LIMIT 1000;

-- ❌ DON'T
SELECT *
FROM error_log_table
ORDER BY error_seq ASC
LIMIT 1000, 1000;

-- ✅ DO
SELECT *
FROM error_log_table
WHERE error_seq > 1000
ORDER BY error_seq ASC
LIMIT 1000;
```
---
### 3. 날짜 필터링 -> range-scan
- 일치 검색 금지
  - Ex) `reg_date = '2025-09-01'`
  - 하나의 값에만 매칭되어 비효율적
  - 실제 값은 '2025-09-01 00:00:00' 하나만 검색
- 함수 사용 금지
  - Ex) `DATE(reg_date) = '2025-09-01'`
  - 함수 적용 없이 원본 컬럼 그대로 사용해야 인덱스 활용 가능
- `BETWEEN` 보다는 `>=, <`사용
  - `reg_date BETWEEN '2025-09-01' AND '2025-09-30'`
    - '2025-09-30 00:00:00'까지만 검색하여, 하루 데이터가 모두 제외됨
  - `reg_date >= '2025-09-01' AND reg_date < '2025-10-01'`
    - 정확히 9월 한 달간의 데이터 탐색
  - 인덱스는 B-Tree 구조이므로, `>=, <`로 탐색 시 **range-scan** -> **가장 효율적**
<br>

Example) 9월 전체 데이터 검색
``` sql
-- 9월 데이터 검색

-- ❌ DON'T
SELECT *
FROM error_log_table
WHERE DATE(reg_date) = '2025-09-01'

-- ❌ DON'T
SELECT *
FROM error_log_table
WHERE reg_date BETWEEN '2025-09-01' AND '2025-09-30'; --9월 30일 누락

-- ✅ DO
SELECT *
FROM error_log_table
WHERE error_seq > 1000
      AND reg_date >= '2025-09-01'
      AND reg_date < '2025-10-01'
ORDER BY error_seq ASC
LIMIT 1000;
```
---
### 4. `SELECT` 절 활용
- `SELECT *` : 모든 컬럼을 가져옴
  - 컬럼 중 TEXT 타입이 있는 경우, **행마다 디스크에서 별도 페이지 읽기 발생**
- 비교적 가벼운 컬럼을 추출 후 범위를 줄여 **단계적 접근**이 필요
<br>

``` sql
-- ✅ DO
SELECT error_seq, reg_date
FROM error_log_table
WHERE error_seq > 1000
      AND reg_date >= '2025-09-01'
      AND reg_date < '2025-10-01'
ORDER BY error_seq ASC
LIMIT 1000;
```
---
# 5. 항상 EXPLAIN으로 실행 계획 확인
- `EXPLAIN`으로 type, key, rows 등을 보고 인덱스 사용 여부 판단
- type = ALL : 풀스캔 -> ❌
- range/ref/const : 인덱스 사용 신호 -> ✅ 
