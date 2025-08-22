# CAST VS CONVERT
1. CAST
- SQL 표준 함수
  - MySQL뿐만 아니라 Oracle, PostgreSQL, SQL Server 등 대부분 DB에서 동일하게 사용 가능
  - 이식성이 높음 (DB를 교체하거나 멀티 DB 지원할 때 유리)
- 가독성이 직관적 (사람이 읽기 좋음)
``` mysql
SELECT CAST('123' AS UNSIGNED);
```
2. CONVERT
- MySQL 고유 함수 (표준 SQL과는 다름)
- CONVERT(expr USING charset) 처럼 문자셋 변환 기능도 지원 (CAST는 불가)
- 문자셋/콜레이션 관련 변환 가능
``` mysql
SELECT CONVERT('123', UNSIGNED);
SELECT CONVERT(name USING utf8mb4);
```
3. CONVERT CAST 공통점
- 형변환을 담당하는 함수
- 성능 차이가 거의 없어 옵티마이저에서 같은 동작으로 취급
4. CONVERT CAST 사용
- CAST: 일반적인 데이터 타입 변환
- CONVERT: mysql에서의 문자셋/콜레이션 변환
