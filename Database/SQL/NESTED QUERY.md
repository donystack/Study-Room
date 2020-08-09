<b>서브쿼리(Subquery)란 하나의 SQL문 안에 포함되어 있는 또 다른 SQL문을 말한다.</b>

조인은 집합간의 곱(Product)의 관계이다. 1:M 관계의 테이블을 조인하면 M(=1*M) 레벨의 집합이 생긴다. M:N 관계의 테이블을 조인하면 MN(=M*N) 레벨의 집합이 결과로서 생성된다.
<b>서브쿼리는 서브쿼리 레벨과는 상관없이 항상 메인쿼리 레벨로 결과 집합이 생성된다.</b>
-->SQL문에서 서브쿼리 방식을 사용해야 할 때 잘못 판단하여 조인 방식을 사용하는 경우가 있다.(ex. 결과는 조직 레벨이고 사원 테이블에서 체크해야 할 조건이 존재할 경우)

<br><br>
<b>서브쿼리의 종류는 동작하는 방식이나 반환되는 데이터의 형태에 따라 분류할 수 있다.</b>

동작하는 방식에 따른 서브쿼리 분류

* Un-Correlated(비연관) 서브쿼리 : 서브쿼리가 메인쿼리 칼럼을 가지고 있지 않는 형태의 서브쿼리이다. 메인쿼리에 값(서브쿼리가 실행된 결과)을 제공하기 위한 목적으로 주로 사용한다.
* Correlated(연관, 상관) 서브쿼리 : 서브쿼리가 메인쿼리 칼럼을 가지고 있는 형태의 서브쿼리이다. 일반적으로 메인쿼리가 먼저 수행되어 읽혀진 데이터를 서브쿼리에서 조건이 맞는지 확인하고자 할 때 주로 사용된다.
(서브쿼리는 메인쿼리 안에 포함된 종속적인 관계이기 때문에 논리적인 실행순서는 항상 메인쿼리에서 읽혀진 데이터에 대해 서브쿼리에서 해당 조건이 만족하는 지를 확인하는 방식으로 수행되어야 한다. 그러나 실제 서브쿼리의 실행순서는 상황에 따라 달라질 수 있다.)

반환되는 데이터의 형태에 다른 서브쿼리 분류

* Single Row 서브쿼리(단일 행 서브쿼리) : 서브쿼리의 실행 결과가 항상 1건 이하인 서브쿼리를 의미한다. 단일 행 서브쿼리는 단일 행 비교 연산자와 함께 사용된다. 단일 행 비교 연산자에는 = , < , <= , > , >= , <>이 있다.
* Multi Row 서브쿼리(다중 행 서브쿼리) : 서브쿼리의 실행 결과가 여러 건인 서브쿼리를 의미한다. 다중 행 서브쿼리는 다중 행 비교 연산자와 함께 사용된다. 다중 행 비교연산자에는 IN, ALL ANY, SOME, EXIST가 있다.


서브쿼리 사용 시 주의사항
1. 서브쿼리를 괄호로 감싸서 사용한다.
2. 서브쿼리는 단일 행(Single Row) 또는 복수 행(Multiple Row) 비교 연산자와 함께 사용 가능하다. 단일 행 비교 연산자는 서브쿼리의 결과가 반드시 1건 이하이어야 하고 복수 행 비교 연산자는 서브쿼리의 결과 건수와 상관 없다.
3. 서브쿼리에서는 ORDER BY를 사용하지 못한다. ORDER BY절은 SELECT절에서 오직 한 개만 올 수 있기 때문에 ORDER BY절은 메인쿼리의 마지막 문장에 위치해야 한다.
4. 서브쿼리는 메인쿼리의 칼럼을 모두 사용할 수 있지만 메인쿼리는 서브쿼리의 칼럼을 사용할 수 없다. 질의 결과에 서브쿼리 칼럼을 표시해야 한다면 조인 방식으로 변환하거나 함수, 스칼라 서브쿼리(Scalar Subquery)등을 사용해야 한다.


<br>서브쿼리가 SQL문에서 사용이 가능한 곳
- SELECT 절 
- FROM 절 (=인라인 뷰)
- WHERE 절
- HAVING 절
- ORDER BY 절
- INSERT문의 VALUES 절
- UPDATE문의 SET 절


CF. 쿼리의 결과가 릴레이션이 아닌 경우에는 그 결과에 따라서 어느 위치든 subquery가 올 수 있다.

<br>

#### 1. 단일 행 서브 쿼리(스칼라 서브쿼리 = 1 Row 1 Column만을 반환하는 서브쿼리)

```
SELECT EMPNAME, TITLE
FROM EMPLOYEE
WHERE TITLE = 
        (SELECT TITLE
        FROM EMPLOYEE
        WHERE EMPNAME = '박영권');
```
만약 동명이인에 박영권이 있다면?  기본키를 조건으로 검색하면 single value가 나온다. 즉 기본키가 아니라면 사용하기 쉽지 않다. 조건이 =이다. 결과가 집합 혹은 릴레이션이면 위험하다 --> 신중하게 사용하여야 한다.

<br>

#### 2. 다중 행 서브 쿼리 
서브쿼리의 결과가 2건 이상 반환될 수 있다면 반드시 다중 행 비교 연산자(IN, ALL, ANY(SOME))와 함께 사용해야 한다.


-------------
1. IN 
- 조건절에서 사용하며 다수의 비교값과 비교하여 비교값 중 하나라도 같은 값이 있다면 true 이다.
- SELECT * FROM emp WHERE sal IN(950, 3000, 1250);
- 'sal = 950 OR sal = 3000 OR sal = 1250' 
- 950, 3000, 1250 과 동일한 값은 모두 출력한다.

2. ANY
- 다수의 비교값 중 한개라도 만족하면 true 이다.
- IN  과 다른점은 비교 연산자를 사용한다는 점이다.

SELECT * FROM emp WHERE sal  =ANY(950, 3000, 1250)
- 이 문장은 위의 IN의 결과와 같다. "=" 연산자는 비교 값과 같은 값은 모두 출력하게 된다.
- 'sal = 950 OR sal = 3000 OR sal = 1250'  와 동일하다. 

SELECT * FROM emp WHERE sal >ANY(950, 3000, 1250)
- 이 문장은 ">"연산자를 사용했다. 이 쿼리의 결과는 950보다 큰 값은 모두 출력하게 된다.
- 'sal > 950 OR sal > 3000 OR sal > 1250'  와 동일하다. 

SELECT * FROM emp WHERE sal <ANY(950, 3000, 1250)
- 이 문장은 "<" 연산자를 사용했다. 이 쿼리의 결과는 3000보다 작은 값은 모두 출력하게 된다.
- 'sal < 950 OR sal < 3000 OR sal < 1250'  와  동일하다. 

결국 ANY는 나올 수 있는 모든 조건에 OR 연산을 수행한것과 동일한 결과를 얻는다.  

3. ALL
- 전체 값을 비교하여 모두 만족해야만 true 이다.

SELECT * FROM emp WHERE sal =ALL(950, 3000, 1250)
- 결과가 하나도 없다. 하나의 값이 모든 값과 일치 할 수 없기 때문이다.
- 'sal = 950 AND sal = 3000 AND sal = 1250' 

SELECT * FROM emp WHERE sal>ALL(950, 3000, 1250)
- 3000보다 큰 값만 표시된다. 'sal > 950 AND sal > 3000 AND sal > 1250' 과 동일하다.

SELECT * FROM emp WHERE sal <font color="#e31600"><</font> ALL(950, 3000, 1250)
- 950보다 작은 값만 표시된다. 'sal < 950 AND sal < 3000 AND sal < 1250' 과 동일하다.

결국 ALL은  나올 수 있는 모든 조건에 AND 연산을 수행한것과 동일한 결과를 얻는다.  

출처: https://carami.tistory.com/18 [carami's story]

-----



#### 3. 다중 칼럼 서브쿼리
쿼리의 결과로 여러 개의 칼럼이 반환되어 메인쿼리의 조건과 동시에 비교되는 것을 의미한다.

예시) 소속팀별 키가 가장 작은 사람들의 정보
```
SELECT TEAM_ID 팀코드, PLAYER_NAME 선수명, POSITION 포지션
  FROM PLAYER
 WHERE (TEAM_ID, HEIGHT) IN (SELECT TEAM_ID ,min(HEIGHT)
                              FROM PLAYER
                             GROUP BY TEAM_ID)
 ORDER BY TEAM_ID, PLAYER_NAME;
```

#### 4. 연관 서브쿼리 : 서브쿼리 내에 메인쿼리 칼럼이 사용된 서브쿼리
중첩 질의의 수행 결과가 단일 값이든, 하나 이상의 애튜리뷰트로 이루어진 릴레이션이든 외부 질의로 한 번만 결과를 반환하면 상관 중첩 질의가 아니다. <b>상관 중첩 질의에서는 외부 질의를 만족하는 각 투플이 구해진 후에 중첩 질의가 수행된다. 따라서 상관 중첩 질의는 외부 질의를 만족하는 투플 수만큼 여러 번 수행될 수 있다.</b>

예시) 선수 자신이 속한 팀의 평균 키보다 작은 선수들의 정보
```
SELECT *
  FROM PLAYER M, TEAM T
 WHERE M.TEAM_ID = T.TEAM_ID
   AND M.HEIGHT < (SELECT AVG(S.HEIGHT)
                     FROM PLAYER S
                    WHERE S.TEAM_ID = M.TEAM_ID
                      AND S.HEIGHT IS NOT NULL
                    GROUP BY S.TEAM_ID)
```

#### EXISTS
<mark>중첩 질의의 결과로 여러 애트리뷰트들로 이루어진 릴레이션이 반환되는 경우</mark>에는 EXISTS 연산자를 사용하여 <mark>중첩 질의의 결과가 빈 릴레이션인지 여부를 검사한다.</mark> EXISTS 연산자는 중첩질의의 결과를 만족하는 값이 존재하는지 여부만 검사하므로 중첩 질의의 SELECT절에 임의의 애트리뷰트들의 리스트가 올 수 있다.(조건을 만족하는 건이 어러 건이라도 1건만 찾으면 더 이상 검색하지 않는다.) 


예시) 영업부나 개발부에 근무하는 사원들의 이름을 검색하라.
````
SELECT EMPNAME
  FROM EMPLOYEE E
 WHERE 
 EXIST (SELECT *
          FROM DEPARTMENT D
         WHERE E.DNO = D.DEPTNO 
           AND (DEPTNAME = '영업' OR DEPTNAME = '개발'));
````
 
#### 5. 그밖에 위치에서 사용하는 서브쿼리

가. SELECT 절에 서브쿼리 사용하기
스칼라 서브쿼리 = 1 Row 1 Column만을 반환하는 서브쿼리
스칼라 서브쿼리는 칼럼을 쓸 수 있는 대부분의 곳에서 사용할 수 있다.

EX) 선수의 소속팀별 평균키를 알아내는 스칼라 서브쿼리(는 메인쿼리의 결과 건수만큼 반복수행 된다.)
SELECT PLATER_NAME 선수명, HEIGHT 키, ROUND( (SELECT AVG(HEIGHT) 
                                              FROM PLAYER X
                                             WHERE X.TEAM_ID = P.TEAM_ID)3) 팀평균키
FROM PLAYER X


나. FROM 절에서 서브쿼리 사용하기
FROM 절에서 사용되는 서브쿼리를 인라인 뷰(Inline View)라고 한다.
서브쿼리의 칼럼은 메인쿼리에서 사용할 수 없다고 했다. 그러나 인라인 뷰는 동적으로 생성된 테이블이다. 인라인 뷰를 사용하는 것은 조인 방식을 사용하는 것과 같다. 그렇기 때문에 인라인 뷰의 칼럼은 SQL문에서 자유롭게 참조할 수 있다.

EX) K-리그 선수들 중에서 포지션이 미드필더(MF)인 선수들의 소속팀명 및 선수 정보(인라인 뷰를 활용해서)
```
SELECT 소속팀명, 선수정보
  FROM TEAM T,
       (SELECT TEAM_ID, PLAYER_NAME, BACK_NO
          FROM PLAYER
         WHERE POSITION = 'MF') P       
WHERE P.TEAM_ID = T.TEAM_ID
ORDER BY 선수명;
```
다. HAVING 절에서 서브쿼리 사용하기

EX) 평균키가 삼성 블루윙즈팀의 평균키보다 작은 팀의 이름과 해당 팀의 평균키
````
SELECT 팀이름, AVG(HEIGHT)
  FROM PLAYER
 GROUP BY TEAM_ID
HAVING AVG(키) < (SELECT AVG(HEIGHT)
                    FROM PLAYER
                   WHERE TEAM_ID = 'K02');
````              

라 UPDATE문의 SET 절에서 사용하기