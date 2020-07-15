<b>UPDATE문에서도 Table Alias를 사용할 수 있다.</b>


Oracle에서는 SELECT, INSERT, UPDATE, DELETE문에서 TABLE ALIAS사용법이 동일하다. Oracle에서의 ALIAS 사용을 이해하기 위해서 MS-SQL에서의 ALIAS 사용법과 비교해 보았다.



```
(Oracle)
UPDATE HOLD_TABLE Q
SET Q.TITLE = 'TEST'
WHERE Q.ID = 101;
```
```
(MS-SQL)
UPDATE Q
SET Q.TITLE = 'TEST'
FROM HOLD_TABLE Q
WHERE Q.ID = 101;
```