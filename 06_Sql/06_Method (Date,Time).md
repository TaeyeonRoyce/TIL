# 시간/날짜 관련 함수

- ### 현재 시간/날짜

| 함수                           | 설명                  |
| ------------------------------ | --------------------- |
| **CURRENT_DATE**, **CURDATE**  | 현재 날짜 반환        |
| **CURRENT_TIME**, **CURTIME**  | 현재 시간 반환        |
| **CURRENT_TIMESTAMP**, **NOW** | 현재 시간과 날짜 반환 |

```sql
SELECT 
	CURDATE(), -- 2021-11-28
	CURTIME(), -- 02:50:46
	NOW();     -- 2021-11-28 02:50:46
```



- ### 시간/날짜 생성

| 함수 | 설명                    |
| ---- | ----------------------- |
| DATE | 문자열에 따라 날짜 생성 |
| TIME | 문자열에 따라 시간 생성 |

```sql
SELECT
  '2021-6-1' = '2021-06-01',            -- 0 (FALSE)
  DATE('2021-6-1') = DATE('2021-06-01'),-- 1 (TRUE)
  '1:2:3' = '01:02:03',									-- 0 (FALSE)
  TIME('1:2:3') = TIME('01:02:03');			-- 1 (TRUE)
```

시간/날짜 형식이 존재함

시간/날짜 형식간의 연산 가능

- Orders 테이블에서 OrderDate가 (1997-1-1) ~ (1997-1-31)인  행의 모든 컬럼 선택

```sql
SELECT * FROM Orders
WHERE
  OrderDate BETWEEN DATE('1997-1-1') AND DATE('1997-1-31');
```



- ### 시간/날짜 형식에서 원하는 값 반환

- 날짜

| 함수                    | 설명                                       |
| ----------------------- | ------------------------------------------ |
| **YEAR**                | 주어진 DATETIME값의 년도 반환              |
| **MONTHNAME**           | 주어진 DATETIME값의 월(영문) 반환          |
| **MONTH**               | 주어진 DATETIME값의 월 반환                |
| **WEEKDAY**             | 주어진 DATETIME값의 요일값 반환(월요일: 0) |
| **DAYNAME**             | 주어진 DATETIME값의 요일명 반환            |
| **DAYOFMONTH**, **DAY** | 주어진 DATETIME값의 날짜(일) 반환          |

```mysql
SELECT
  OrderDate,
  YEAR(OrderDate) AS YEAR,
  MONTHNAME(OrderDate) AS MONTHNAME,
  MONTH(OrderDate) AS MONTH,
  WEEKDAY(OrderDate) AS WEEKDAY,
  DAYNAME(OrderDate) AS DAYNAME,
  DAY(OrderDate) AS DAY
FROM Orders;
```

| OrderDate  | YEAR | MONTHNAME | MONTH | WEEKDAY | DAYNAME  | DAY  |
| :--------- | :--- | :-------- | :---- | :------ | :------- | :--- |
| 1996-07-04 | 1996 | July      | 7     | 3       | Thursday | 4    |
| 1996-07-05 | 1996 | July      | 7     | 4       | Friday   | 5    |
| 1996-07-08 | 1996 | July      | 7     | 0       | Monday   | 8    |



- Orders테이블에서 OrderDate의 요일이 월요일인 행의 모든 컬럼

```sql
SELECT * FROM Orders
WHERE WEEKDAY(OrderDate) = 0;
```



- 시간

| 함수       | 설명                      |
| ---------- | ------------------------- |
| **HOUR**   | 주어진 DATETIME의 시 반환 |
| **MINUTE** | 주어진 DATETIME의 분 반환 |
| **SECOND** | 주어진 DATETIME의 초 반환 |

```Sql
SELECT
  HOUR(NOW()) AS HOUR,
  MINUTE(NOW()) AS MINUTE,
  SECOND(NOW())  AS SECOND;
```

| HOUR | MINUTE | SECOND |
| :--- | :----- | :----- |
| 2    | 57     | 40     |



- ### 시간/날짜 다루기

| 함수                      | 설명             |
| ------------------------- | ---------------- |
| **ADDDATE**, **DATE_ADD** | 시간/날짜 더하기 |
| **SUBDATE**, **DATE_SUB** | 시간/날짜 빼기   |

```sql
SELECT 
  ADDDATE('2021-06-20', INTERVAL 1 YEAR),  -- 날짜 +1년
  ADDDATE('2021-06-20', INTERVAL -2 MONTH),-- 날짜 -2달
  ADDDATE('2021-06-20', INTERVAL 3 WEEK),  -- 날짜 +3주
  ADDDATE('2021-06-20', INTERVAL -4 DAY),  -- 날짜 -4일
  ADDDATE('2021-06-20', INTERVAL -5 MINUTE),-- 날짜 -5분
  ADDDATE('2021-06-20 13:01:12', INTERVAL 6 SECOND); -- 날짜 +6초
```



| 함수          | 설명                   |
| ------------- | ---------------------- |
| **DATE_DIFF** | 두 시간/날짜 간 일수차 |
| **TIME_DIFF** | 두 시간/날짜 간 시간차 |



- Orders테이블에서 OrderDate, 현재 날짜/시간 (NOW()), OrderDate와 현재 날짜의 일수차의 절대값 컬럼 선택

```sql
SELECT
  OrderDate,
  NOW(),
  ABS(DATEDIFF(OrderDate, NOW()))
FROM Orders;
```

| OrderDate  | NOW()               | ABS(DATEDIFF(OrderDate, NOW())) |
| :--------- | :------------------ | :------------------------------ |
| 1996-07-04 | 2021-11-28 03:10:29 | 9278                            |
| 1996-07-05 | 2021-11-28 03:10:29 | 9277                            |
| 1996-07-08 | 2021-11-28 03:10:29 | 9274                            |



- ### DATE_FORMAT

| 함수        | 설명                             |
| ----------- | -------------------------------- |
| DATE_FORMAT | 시간/날짜를 지정한 형식으로 반환 |

| 형식           | 설명                      |
| -------------- | ------------------------- |
| **%Y**         | 년도 4자리                |
| **%y**         | 년도 2자리                |
| **%M**         | 월 영문                   |
| **%m**         | 월 숫자                   |
| **%D**         | 일 영문(1st, 2nd, 3rd...) |
| **%d**, **%e** | 일 숫자 (01 ~ 31)         |
| **%T**         | hh:mm:ss                  |
| **%r**         | hh:mm:ss AM/PM            |
| **%H**, **%k** | 시 (~23)                  |
| **%h**, **%l** | 시 (~12)                  |
| **%i**         | 분                        |
| **%S**, **%s** | 초                        |
| **%p**         | AM/PM                     |

```sql
SELECT
  DATE_FORMAT(NOW(), '%M %D, %Y %T'),     		 -- November 28th, 2021 03:12:58
  DATE_FORMAT(NOW(), '%y-%m-%d %h:%i:%s %p'),  -- 21-11-28 03:12:58 AM
  DATE_FORMAT(NOW(), '%Y년 %m월 %d일 %p %h시 %i분 %s초'); -- 	2021년 11월 28일 AM 03시 12분 58초
```



| 함수                      | 설명                                  |
| ------------------------- | ------------------------------------- |
| **STR _ TO _ DATE**(S, F) | S를 F형식으로 해석하여 시간/날짜 생성 |

```sql
SELECT
  OrderDate,
  DATEDIFF(
    STR_TO_DATE('1997-01-01 13:24:35', '%Y-%m-%d %T'),
    OrderDate
  ) AS Day_DIFF_initOrder
FROM Orders;
```

| OrderDate  | Day_DIFF_initOrder |
| :--------- | :----------------- |
| 1996-07-04 | 181                |
| 1996-07-05 | 180                |
| 1996-07-08 | 177                |