### 실행계획 확인하기
실행계획을 확인하는 방법은 여러가지가 있다.(오라클)
1. SQL Developer에서는 SQL을 작성한 후에 F10키
2. Toad에서는 SQL을 작성한 후에 Ctrl+E 키
3. EXPLAIN PLAN FOR 명령어 사용한 다음 SELECT * FROM TABLE(DBMS_XPLAN.DISPLAY())


정확한 순서보다는 실행계획의 트리를 통해 흐름을 파악하는 것이 중요하다.


### 실제 실행계획 확인하기
실제 실행계획을 확인하는 방법은 2가지가 있다.
1. SQL에 'GATHER_PLAN_STATISTICS' 힌트를 사용.
2. 세션에 트레이스(Trace)를 걸기.

GATHER_PLAN_STATISTICS 힌트를 추가해 SQL을 실행하면, 자세한 실행 정보가 오라클 내에 저장된다. 힌트가 포함된 SQL이 실행 완료된 후에는 DBMS_XPLAN.DISPLAY_CURSOR를 이용해 실행계획을 확인할 수 있다.

SQL에 'GATHER_PLAN_STATISTICS' 힌트를 사용해 보자.
```
SELECT  /*+ GATHER_PLAN_STATISTICS */
FROM    T_ORD T1
        ,M_CUS T2
WHERE   T1.CUS_ID = T2.CUS_ID
AND     T2.CUS_GD = 'A';
```

그 다음 실제 실행계획을 만든 SQL의 SQL_ID찾아낸다.
```
SELECT  T1.SQL_ID ,T1.CHILD_NUMBER ,T1.SQL_TEXT 
FROM    V$SQL T1
WHERE   T1.SQL_TEXT LIKE '%GATHER_PLAN_STATISTICS%'
ORDER BY T1.LAST_ACTIVE_TIME DESC;
```
이 때 나의 경우에는 ORA-00942: table or view does not exist V$SQL 에러가 나왔고 링크에서 말한 방법으로 해결할 수 있었다. https://stackoverrun.com/ko/q/2661208


그 다음 찾아낸 SQL_ID와 CHILD_NUMBER를 이용해 아래 SQL을 실행한다.
```
SELECT  *
FROM TABLE(DBMS_XPLAN.DISPLAY_CURSOR('bmjjk7adpg82g',0,'ALLSTATS LAST'));
```

### 정확한 테스트를 위해
```
ALTER SYSTEM FLUSH BUFFER_CACHE;
```
SQL 실행 전에, 위의 명령어로 버퍼캐시에 있는 블록들을 비워내야 한다. 버퍼캐시에 데이터가 이미 존재하고 있으면 실행 시간 측정이 정확히 이루어지지 않는다. 다만 운영 환경에서 실행하면 절대 안 되는 위험한 명령어이다.

 ### V$SQL 뷰 비우기
 ```
ALTER SYSTEM FLUSH SHARED_POOL;
```