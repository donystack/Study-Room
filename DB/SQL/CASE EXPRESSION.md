### CASE 표현

CASE 표현은 IF-THEN-ELSE 논리와 유사한 방식으로 표현식을 작성해서 SQL의 비교 연산 기능을 보완하는 역할을 한다. ANSI/ISO 표준에는 CASE Expression이라고 표시되어 있는데, 함수와 같은 성격을 가지고 있으며 Oracle의 Decode 함수와 같은 기능을 하므로 단일행 내장 함수에서 같이 설명한다.

CASE Expressions은 두 가지 표현법이 있다.
1. Simple Case Expression
2. Searched Case Expression

<BR>
<B>Simple Case Expressiond</B>

<B>CASE 다음에 바로 조건에 사용되는 칼럼이나 표현식을 표시</B>하고, 다음 WHEN 절에서 앞에서 정의한 칼럼이나 표현식과 같은지 아닌지 판단하는 문장으로 EQUI(=) 조건만 사용한다면 SEARCHED_CASE_EXPRESSION보다 간단하게 사용할 수 있는 장점이 있다. Oracle의 DECODE 함수와 기능면에서 동일하다.

```
CASE 
     EXPR WHEN COMPARISON_EXPR THEN RETURN_EXPR
     ELSE 표현절
 END
```

EX) 부서 정보에서 부서 위치를 미국의 동부, 중부, 서부로 구분하라.
```
SELECT LOC,
       CASE LOC WHEN 'NEW YOURK' THEN 'EAST'
                WHEN 'BOSTON'    THEN 'EAST'
                WHEN 'CHICAGO'   THEN 'CENTER'
                ELSE 'ETC'
        END AS AREA
 FROM DEPT
```

<BR><BR>
<B>Searched Case Expression </B>

CASE 다음에는 칼럼이나 표현식을 표시하지 않고, 다음 WHEN절에서 EQUI(=) 조건 포함 여러 조건(>, >=, <, <=)을 이용한 조건절을 사용할 수 있기 때문에 SIMPLE_CASE_EXPRESSION보다 훨씬 다양한 조건을 적용할 수 있는 장점이 있다.

```
CASE 
     WHEN CONDITION THEN RETURN)EXPR
     ELSE 표현절
END
```
EX) 사원 정보에서 급여가 3000 이상이면 상등급으로, 1000 이상이면 중등급으로, 1000 미만이면 하등급으로 분류하라.
```
SELECT ENAME,
       CASE WHEN SAL >= 3000 THEN 'HIGH'
            WHEN SAL >= 1000 THEN 'MID'
            ELSE 'LOW'
        END AS SALARY_GRADE
  FROM EMP
```


<B>Decode</B>

Oracle에서만 사용되는 함수로, 표현식의 값이 기준값1이면 값1을 출력하고, 기준값2이면 값2를 출력한다. 그리고 기준값이 없으면 디폴트 값을 출력한다. CASE 표현의 SIMPLE_CASE_EXPRESSION 조건과 동일하다. 

참조 : SQL 전문가 가이드 2013 Edition - 한국데이터진흥원