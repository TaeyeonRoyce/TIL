# 문자 관련 함수

- ### 대소문자

| 함수                 | 설명          |
| -------------------- | ------------- |
| **UCASE**, **UPPER** | 모두 대문자로 |
| **LCASE**, **LOWER** | 모두 소문자로 |

```sql
SELECT
  UPPER('abcDEF'), -- ABCDEF
  LOWER('abcDEF'); -- abcdef
```



- ### 문자열 붙이기

| 함수                  | 설명                               |
| --------------------- | ---------------------------------- |
| **CONCAT**(...)       | 괄호 안의 내용 이어붙임            |
| **CONCAT_WS**(S, ...) | 괄호 안의 내용 S로 이어붙임        |
| **LPAD**(S, N, P)     | S가 N글자가 될 때까지 P를 이어붙임 |
| **RPAD**(S, N, P)     | S가 N글자가 될 때까지 P를 이어붙임 |

- Concat, Concat_ws

```sql
SELECT CONCAT('HELLO', ' ', 'THIS IS ', 2021)
-- HELLO,  , This is , 2021을 모두 이어 붙임
-- HELLO THIS IS 2021
```

```sql
SELECT CONCAT_WS('-', 2021, 8, 15, 'AM')
-- 2021-8-15-AM
-- '-'로 모두 이어 붙임
```

- Orders 테이블에서 OrderID값 앞에 'O-ID: ' 붙힌 컬럼을 선택

```sql
SELECT CONCAT('O-ID: ', OrderID) FROM Orders;
```

| CONCAT('O-ID: ', OrderID) |
| :------------------------ |
| O-ID: 10248               |
| O-ID: 10249               |



- LPAD, RPAD

```sql
SELECT
  LPAD('ABC', 5, '-'), -- --ABC
  RPAD('ABC', 5, '-'); -- ABC--
```



- ### 문자열 자르기

| 함수                      | 설명                         |
| ------------------------- | ---------------------------- |
| **SUBSTR**, **SUBSTRING** | 주어진 값에 따라 문자열 자름 |
| **LEFT**                  | 왼쪽부터 N글자               |
| **RIGHT**                 | 오른쪽부터 N글자             |

```sql
SELECT
  SUBSTR('ABCDEFG', 3),  -- CDEFG
  SUBSTR('ABCDEFG', 3, 2), -- CD
  SUBSTR('ABCDEFG', -4),  -- DEFG
  SUBSTR('ABCDEFG', -4, 2); -- DE
```

`SUBSTR('ABCDEFG', 3, 2)`의 경우, 앞에서부터 3자리를 자른 뒤 2자리의 문자

`SUBSTR('ABCDEFG', -4, 2)`의 경우, 뒤에서부터 4자리를 자른 뒤 2자리의 문자

```sql
SELECT
  LEFT('ABCDEFG', 3),  -- ABC
  RIGHT('ABCDEFG', 3); -- EFG
```

- Orders 테이블에서 OrderDate와 OrderDate의 왼쪽 4자리는 Year, OrderDate의 6자리부터 2자리의 문자는 Month, OrderDate의 오른쪽 2자리는 Day라는 컬럼을 선택

```sql
SELECT
  OrderDate,
  LEFT(OrderDate, 4) AS Year,
  SUBSTR(OrderDate, 6, 2) AS Month,
  RIGHT(OrderDate, 2) AS Day
FROM Orders;
```

| OrderDate  | Year | Month | Day  |
| :--------- | :--- | :---- | :--- |
| 1996-07-04 | 1996 | 07    | 04   |
| 1996-07-05 | 1996 | 07    | 05   |
| 1996-07-08 | 1996 | 07    | 08   |



- ### 문자열의 길이

| 함수                                  | 설명                 |
| ------------------------------------- | -------------------- |
| **CHAR_LENGTH**, **CHARACTER_LEGNTH** | 문자열의 문자 길이   |
| **LENGTH**                            | 문자열의 바이트 길이 |

LENGTH는 바이트의 길이를 가져오기 때문에, 영어 외의 사용에서는 CHAR_LENGTH을 자주 씀

```	sql
SELECT
  LENGTH('ABCDE'), 			  -- 5
  CHAR_LENGTH('ABCDE'),   -- 5
  LENGTH('안녕하세요'), 		  -- 15
  CHAR_LENGTH('안녕하세요'), -- 5
```



- ### 문자열 공백 제거

| 함수      | 설명             |
| --------- | ---------------- |
| **TRIM**  | 양쪽 공백 제거   |
| **LTRIM** | 왼쪽 공백 제거   |
| **RTRIM** | 오른쪽 공백 제거 |

- Categories테이블에서 CategoryName의 값이 'Beverages'(' Beverages '아님) 인 행의 모든 컬럼 선택

```sql
SELECT * FROM Categories
WHERE CategoryName = TRIM(' Beverages ')
```



| 함수                 | 설명             |
| -------------------- | ---------------- |
| **REPLACE(S, A, B)** | S중 A를 B로 변경 |

