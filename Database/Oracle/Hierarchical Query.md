### 계층형 질의

계층형 데이터란 동일 테이블에 계층적으로 상위와 하위 데이터가 포함된 데이터를 말한다.

Oracle은 계층형 질의 질의 구문을 제공한다.
```
SELECT ...
FROM 테이블
WHERE condition AND condition ...
START WITH condition
CONNECT BY [NOCYCLE] condition AND condition ...
[ORDER SIBLINGS BY column, column, ...]
```
- START WITH 절은 계층 구조 전개의 시작 위치를 지정하는 구문이다. 즉, 루트 데이터를 지정한다.
- CONNECT BY 절은 다음에 전개될 자식 데이터를 지정하는 구문이다. 자식 데이터는 CONNECT BY절에 주어진 조건을 만족해야 한다.(조인)
- PRIOR : CONNECT BY절에 사용되며, 현재 읽은 칼럼을 지정한다. <b>PRIOR 자식 = 부모 형태를 사용하면 계층구조에서 부모 데이터에서 자식 데이터(부모 -> 자식) 방향으로 전개하는 순방향 전개를 한다. 그리고 PRIOR 부모 = 자식 형태를 사용하면 반대로 자식 데이터에서 부모 데이터(자식 -> 부모) 방향으로 전개하는 역방향 전개를 한다.</b>
- NOCYCLE : 데이터를 전개하면서 이미 나타났던 동일한 데이터가 전개 중에 다시 나타난다면 이것을 가리켜 사이클(Cycle)이 형성되었다라고 말한다. 사이클이 발생한 데이터는 런타임 오류가 발생한다. 그렇지만 NOCYCLE를 추가하면 사이클이 발생한 이후의 데이터는 전개하지 않는다.
- ORDER SIBLINGS BY : 형제 노드(동일 LEVEL) 사이에서 정렬을 수행한다.
- WHERE : 모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 추출한다.(필터링)



EX) 사원 '7876'로부터 자신의 상위관리자를 찾는 역방향 전개
```
 SELECT LEVEL, LPAD(' ', 4 * (LEVEL-1)) || EMPNO 사원,
        MANAGER 관리자, CONNECT_BY_ISLEAF ISLEAP
   FROM EMP
  START WITH EMPNO = '7876'
CONNECT BY PRIOR MGR = EMPNO;
```

EX) 최상위 관리자로부터 하위 관리자를 찾는 순방향 전개
```
 SELECT LEVEL, LPAD(' ', 4 * (LEVEL-1)) || EMPNO 사원,
        MGR 관리자, CONNECT_BY_ISLEAF ISLEAP
   FROM EMP
  START WITH MGR IS NULL
CONNECT BY PRIOR EMPNO = MGR;
```