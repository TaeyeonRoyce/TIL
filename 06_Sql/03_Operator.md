# Select Operator

### 사칙 연산

| 연산자                        | 의미                                      |
| ----------------------------- | ----------------------------------------- |
| **+**, **-**, *****, *, %*/** | 각각 더하기, 빼기, 곱하기, 나누기, 나머지 |

>문자열에 사칙연산을 더 하면 문자열은 0으로 인식
>
>```sql
>SELECT 'ABC' + 3;
>```
>
>| 'ABC' + 3 |
>| :-------- |
>| 3         |
>
>숫자인 문자열에 더하면 숫자로 인식
>
>```sql
>SELECT '1.1' + 3;
>```
>
>| '1.1' + 3 |
>| :-------- |
>| 4.1       |

실 사용

- Products 테이블에서 ProductName과 Price 컬럼의 값을 2로 나눈 값을 HalfPrice라는 컬럼으로 선택하기

```sql
SELECT
  ProductName,
  Price / 2 AS HalfPrice
FROM Products;
```

| ProductName   | HalfPrice |
| :------------ | :-------- |
| Chais         | 9.000000  |
| Chang         | 9.500000  |
| Aniseed Syrup | 5.000000  |



### 논리 연산자

- TURE = 1
- FALSE = 0

```sql
SELECT 0 = TRUE, 1 = TRUE, 0 = FALSE, 1 = FALSE;
```

| 0 = TRUE | 1 = TRUE | 0 = FALSE | 1 = FALSE |
| :------- | :------- | :-------- | :-------- |
| 0        | 1        | 1         | 0         |



- IS

| 연산자     | 의미                        |
| ---------- | --------------------------- |
| **IS**     | 양쪽이 모두 TRUE 또는 FALSE |
| **IS NOT** | 한쪽은 TRUE, 한쪽은 FALSE   |



- AND, OR

| 연산자           | 의미                         |
| ---------------- | ---------------------------- |
| **AND**, **&&**  | 양쪽이 모두 TRUE일 때만 TRUE |
| **OR**, **\|\|** | 한쪽은 TRUE면 TRUE           |



- OrderDetails테이블의 ProductId = 20 이고, ((OrderId = 10514 이거나 Quantity = 50)) 인 행의 모든 열 선택

```sql
SELECT * FROM OrderDetails
WHERE
  ProductId = 20
  AND (OrderId = 10514 OR Quantity = 50);
```

| OrderDetailID | OrderID | ProductID | Quantity |
| :------------ | :------ | :-------- | :------- |
| 697           | 10514   | 20        | 39       |
| 1828          | 10953   | 20        | 50       |



### 비교 연산자

| 연산자         | 의미                             |
| -------------- | -------------------------------- |
| **=**          | 양쪽 값이 같음 ( '=' 하나임)     |
| **!=**, **<>** | 양쪽 값이 다름 ('<>'도 있음)     |
| **>**, **<**   | (왼쪽, 오른쪽) 값이 더 큼        |
| **>=**, **<=** | (왼쪽, 오른쪽) 값이 같거나 더 큼 |

> SQL의 기본 연산자는 대소문자 구분하지 않음
>
> ```sql
> SELECT 'A' = 'a';
> -- 1 (TRUE)
> ```



### 범위 연산자

- BETWEEN

| 연산자                              | 의미                        |
| ----------------------------------- | --------------------------- |
| **BETWEEN** {MIN} **AND** {MAX}     | 두 값 사이에 있음           |
| **NOT BETWEEN** {MIN} **AND** {MAX} | 두 값 사이가 아닌 곳에 있음 |

- OrderDetails테이블 ProductID가 1...4인 행의 모든 열 선택

```sql
SELECT * FROM OrderDetails
WHERE ProductID BETWEEN 1 AND 4;
```

| OrderDetailID | OrderID | ProductID | Quantity |
| :------------ | :------ | :-------- | :------- |
| 21            | 10255   | 2         | 20       |
| 100           | 10285   | 1         | 45       |
| 110           | 10289   | 3         | 30       |



- IN

| 연산자           | 의미                       |
| ---------------- | -------------------------- |
| **IN** (...)     | 괄호 안의 값들 가운데 있음 |
| **NOT IN** (...) | 괄호 안의 값들 가운데 없음 |



- Customers테이블에서 City가 ('Torino', 'Paris', 'Portland', 'Madrid') 중 하나인 행의 모든 열 선택

```sql
SELECT * FROM Customers
WHERE City IN ('Torino', 'Paris', 'Portland', 'Madrid') 
```

| CustomerID | CustomerName                         | ContactName   | Address             | City     | PostalCode | Country |
| :--------- | :----------------------------------- | :------------ | :------------------ | :------- | :--------- | :------ |
| 8          | Bólido Comidas preparadas            | Martín Sommer | C/ Araquil, 67      | Madrid   | 28023      | Spain   |
| 22         | FISSA Fabrica Inter. Salchichas S.A. | Diego Roel    | C/ Moralzarzal, 86  | Madrid   | 28034      | Spain   |
| 27         | Franchi S.p.A.                       | Paolo Accorti | Via Monte Bianco 34 | Torino   | 10100      | Italy   |
| 48         | Lonesome Pine Restaurant             | Fran Wilson   | 89 Chiaroscuro Rd.  | Portland | 97219      | USA     |



### LIKE

| 연산자                   | 의미                          |
| ------------------------ | ----------------------------- |
| **LIKE** '... **%** ...' | 0~N개 문자를 가진 패턴        |
| **LIKE** '... **_** ...' | _ 갯수만큼의 문자를 가진 패턴 |

검색할 때 유용하게 사용될 것 같다.

```sql
LIke '%est'
-- est로 끝나는 단어

LIke '%est%'
-- est가 포함된 단어

LIke 'est%'
-- est로 시작하는 단어

LIke '__est'
-- est로 끝나는 5자리 단어

LIke 'est___'
-- est로 시작하는 6자리 단어
```

- Employees 테이블의 Notes 컬럼의 값이 'economics'을 포함한 행의 모든 컬럼을 선택

```sql
SELECT * FROM Employees
WHERE Notes LIKE '%graduate%'
```

| EmployeeID | LastName | FirstName | BirthDate  | Photo      | Notes                                                        |
| :--------- | :------- | :-------- | :--------- | :--------- | :----------------------------------------------------------- |
| 6          | Suyama   | Michael   | 1963-07-02 | EmpID6.pic | Michael is a **graduate** of Sussex University (MA, economics) and the University of California at Los Angeles (MBA, marketing). |
| 5          | Buchanan | Steven    | 1955-03-04 | EmpID5.pic | Steven Buchanan **graduated** from St. Andrews University, Scotland, with a BSC degree. |



- OrderDetails 테이블의 OrderID 컬럼의 값이 `1025로 시작하는 5자리(LIKE '1025_')`인 행의 모든 컬럼을 선택

```sql
SELECT * FROM OrderDetails
WHERE OrderID LIKE '1025_'
```

| OrderDetailID | OrderID | ProductID | Quantity |
| :------------ | :------ | :-------- | :------- |
| 6             | 10250   | 41        | 10       |
| 7             | 10250   | 51        | 35       |
| 8             | 10250   | 65        | 15       |
| 9             | 10251   | 22        | 6        |
| 10            | 10251   | 57        | 15       |

