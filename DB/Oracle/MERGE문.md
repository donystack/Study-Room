### MERGE문 기본 구조

```
MERGE INTO [스키마, ] 테이블명
USING (update나 insert될 데이터 원천)
ON (update될 조건)

WHEN MATCHED THEN
SET 컬럼1 = 값1, 컬럼2 = 값2, ....

WHEN NOT MATCHED THEN
INSERT (칼럼1, 칼럼2, ....) VALUES (값1, 값2, ...)
WHERE insert 조건;
```

