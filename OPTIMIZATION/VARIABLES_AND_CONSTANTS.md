# VARIABLES AND CONSTANTS
1. 데이터 타입이 서로 다른 컬럼과 상수를 비교하는 경우
- 컬럼을 형변환하기보다 상수를 형변환하는 것이 옳다
- 컬럼값을 형변환하는 경우 매 로우마다 형변환이 발생하기 때문에 풀스캔, CPU소모
- 상수를 형변환하는 경우 이미 형변환한 상수를 매번 사용하기 때문에 훨씬 성능이 좋음
``` mysql
SELECT *
FROM user
WHERE CAST(user_id AS CHAR) = '123';
```
- 풀스캔으로 모든 로우마다 형변환하여 CPU를 낭비한다
- user_id에 걸려있는 인덱스를 무시한다
``` mysql
SELECT *
FROM user
WHERE user_id = CAST('text' AS SIGNED);
```
- '123'이라는 값을 쿼리 실행 전에 정수로 형변환
- user_id를 그대로 사용하여 CPU소모 감소

