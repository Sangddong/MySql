# 대용량 테이블에서 효율적으로 검색하는 방법
``` sql
CREATE TABLE error_log_table (
  error_seq INT PRIMARY KEY AUTO_INCREMENT,
  api VARCHAR(100) NOT NULL,
  payload TEXT NOT NULL,
  reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
### 1. `WHERE`절 
- `WHERE`절은 **입력된 순서대로 필터링**이 걸리게 되므로
- **인덱스와 관련이 깊은 컬럼을 선순위**로 두어 **풀스캔 방지**
Example)
``` sql
-- ❌ DON'T
SELECT *
FROM error_log_table
WHERE payload like '%example%';

-- ✅ DO
SELECT *
FROM error_log_table
WHERE error_seq BETWEEN 10000 AND 20000
      AND reg_date >= '2025-09-01' AND reg_date < '2025-10-01';
```

### 2. `OFFSET`보다 `SEEK`방식
- `OFFSET N LIMIT M` : DB가 앞의 N개 행을 건너뛰기 위해 스캔 -> 비용이 O(N + M) -> **매우 느림**
- `SEEK`방식은 키셋 페이징 방식으로,
- 마지막으로 본 행의 정렬 키 값을 기준으로 다음 페이지를 가져오는 방식
Example)
``` sql
-- ❌ DON'T
SELECT *
FROM error_log_table
ORDER BY error_seq ASC
OFFSET 1000 LIMIT 1000;

-- ✅ DO
SELECT *
FROM error_log_table
WHERE error_seq > 1000
ORDER BY error_seq ASC
LIMIT 1000;
```
