# COLLATION
#### 1. CHRACTER SET
- 문자 인코딩 규칙
- 어떤 글자를 저장할 수 있는지 결정
#### 2. COLLATION
- 정렬, 비교 규칙
``` mysql
-- utf8mb4_general_ci (대소문자 구분 안 함)
SELECT 'a' = 'A';  -- 결과: 1 (TRUE)

-- utf8mb4_bin (이진값 비교 -> 대소문자 구분)
SELECT 'a' = 'A';  -- 결과: 0 (FALSE)
```
