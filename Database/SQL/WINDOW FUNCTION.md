#### WINDOW 함수는 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수

 WINDOW 함수의 종류

- 순위(RANK) 관련 함수 : RANK, DENSE_RANK, ROW_NUMBER (ANSI/ISO)
- 그룹 내 집계(AGGREGATE) 관련 함수 : SUM, MAX, MIN ,AVG, COUNT (ANSI/ISO)
- 그룹 내 행 순서(ORDER) 관련 함수 : FIRST_VALUE, LAST_VALUE, LAG, LEAD (Oracle)
- 그룹 내 비율(RATIO) 관련 함수 : CUME_DIST, PERCENT_RANK,  RATIO_TO_REPORT (ANSI/ISO) NTILE(Oracle, MS-SQL)


함수 기본 구조 (OVER는 필수 키워드이다)
```
SELECT WINDOW_FUNCTION(ARGUMENTS) OVER ([PARTITION BY 칼럼] [ORDER BY 절] [WINDOWING 절])

  FROM 테이블 명 ;
```

[예제] 사원 데이터에서 급여가 높은 순서와 JOB 별로 급여가 높은 순서를 같이 출력한다.
```
SELECT JOB, ENAME, SAL,
       RANK() OVER (ORDER BY SAL DESC) ALL RANK,
       RANK() OVER (PARTITION BY JOB ORDER BY SAL DESC) JOB_RANK
  FROM EMP;
```

- ARGUMENTS(인수) : 함수에 따라 0 ~ N개의 인수가 지정될 수 있다.
- PARTITION BY 절 : 전체 집합을 기준에 의해 소그룹으로 나눌 수 있다.
- ORDER BY 절 : 어떤 항목에 대해 순위를 지정할 지 ORDER BY 절을 기술한다.
- WINDOWING 절 : WINDOWING 절은 함수의 대상이 되는 행 기준의 범위를 강력하게 지정할 수 있다. ROWS는 물리적인 결과 행의 수를, RANGE는 논리적인 값에 의한 범위를 나타내는데, 둘 중의 하나를 선택해서 사용할 수 있다. 다만 WINDOWING 절은 MS-SQL에서는 지원하지 않는다.

```
ROWS | RANGE BETWEEN
    UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING 
AND
    UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING/FOLLOWING

BETWEEN 미사용 타입
ROWS | RANGE
    UNBOUNDED PRECEDING | CURRENT ROW | VALUE_EXPR PRECEDING
```

[예제] EMP 테이블에서 같은 매니저를 두고 있는 사원들의 평균 SALARY를 구하는데, 조건은 같은 매니저 내에서 자기 바로 앞의 사번과 바로뒤의 사번인 직원만을 대상으로 한다.
```
SELECT MGR, ENAME, HIREDATE, SAL, 
       ROUND( AVG(SAL) OVER (PARTITION BY MGR ORDER BY HIREDATE
              ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING)) AS MGR_AVG
  FROM EMP;
```

