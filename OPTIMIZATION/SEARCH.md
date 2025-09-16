```sql
CREATE TABLE t_nf_log (
  err_sec INT PRIMARY KEY AUTO_INCREMENT,
  payload TEXT,                 -- JSON string
  api VARCHAR(100),
  reg_date TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```
1. `payload`가 `TEXT` 타입이라 직접 검색하기가 느림 → JSON 파싱 불가 + FULLTEXT 미사용 시 인덱스 불가
2. `api`, `reg_date`는 자주 조회될 가능성이 있음 → 인덱스 필요
3. PK(`err_sec`)만 있어 range search 외에는 빠른 탐색 어려움

---

## 🚀 효율적인 검색 전략

### 1. **쿼리 패턴 분석**

* "특정 기간의 로그 조회" → `WHERE reg_date BETWEEN ...`
* "특정 API 요청 로그 조회" → `WHERE api = 'xxx'`
* "payload 내부 특정 필드 값 검색" → JSON 파싱 or 문자열 LIKE 검색

👉 어떤 검색이 더 많은지에 따라 인덱스를 최적화해야 합니다.

---

### 2. **인덱스 최적화**

* 기본적으로 아래 인덱스를 고려하세요.

```sql
CREATE INDEX idx_api ON t_nf_log(api);
CREATE INDEX idx_reg_date ON t_nf_log(reg_date);
CREATE INDEX idx_api_reg_date ON t_nf_log(api, reg_date); -- 복합 인덱스
```

* `api` + `reg_date` 조합 검색이 많으면 복합 인덱스가 특히 유용합니다.

---

### 3. **Payload(JSON) 처리 전략**

현재 `payload TEXT`에는 JSON이 문자열로 들어갑니다.
이 경우 효율적인 검색이 어렵습니다. 해결 방법:

#### (1) MySQL 5.7 이상 → `JSON` 타입 활용

```sql
ALTER TABLE t_nf_log 
MODIFY payload JSON;
```

* 그러면 `JSON_EXTRACT(payload, '$.field')` + **generated column + index** 가능

예:

```sql
ALTER TABLE t_nf_log 
ADD COLUMN user_id VARCHAR(50) GENERATED ALWAYS AS (json_unquote(json_extract(payload, '$.userId'))) STORED,
ADD INDEX idx_user_id (user_id);
```

#### (2) TEXT 유지 시

* FULLTEXT index: payload 전체 텍스트 검색 가능 (단, 정확한 JSON field 검색은 어려움)

```sql
ALTER TABLE t_nf_log ADD FULLTEXT INDEX idx_payload (payload);
```

* 단, LIKE '%...%'는 여전히 느림

---

### 4. **파티셔닝 고려**

20M 데이터는 인덱스만으로는 한계가 있음 → `파티셔닝` 고려

* **범위 파티셔닝** (주로 날짜 기준)

```sql
ALTER TABLE t_nf_log
PARTITION BY RANGE (YEAR(reg_date)) (
  PARTITION p2023 VALUES LESS THAN (2024),
  PARTITION p2024 VALUES LESS THAN (2025),
  PARTITION pmax VALUES LESS THAN MAXVALUE
);
```

👉 이렇게 하면 특정 연도의 데이터 검색 시 불필요한 스캔을 피할 수 있습니다.

---

### 5. **운영 전략**

* **주기적 아카이빙**: 오래된 데이터는 다른 테이블로 옮기거나 압축
* **쿼리 캐싱**: 동일 검색 반복 시 Redis 같은 캐시 활용
* **페이징 최적화**: `LIMIT offset, size` 대신 "seek 방식" (`WHERE id > last_seen_id LIMIT size`)

---

✅ 정리

1. `api`, `reg_date` 컬럼에 인덱스 + 복합 인덱스
2. `payload`는 가능하면 `JSON` 타입으로 바꾸고, generated column + 인덱스 활용
3. 데이터가 너무 크면 `파티셔닝` 고려 (보통 날짜 기준)
4. 검색 패턴에 따라 캐싱·아카이빙 전략도 함께 운영

---

👉 혹시 실제로 검색하려는 조건이 `payload` 안의 특정 키 값인가요, 아니면 주로 `api`와 `날짜` 기준인가요? 이거에 따라 설계 방향이 완전히 달라집니다.
