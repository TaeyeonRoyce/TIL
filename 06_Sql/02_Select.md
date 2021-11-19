# Select



### 테이블 전체 조회하기

- Customer테이블의 모든 컬럼 가져오기

```sql
SELECT * FROM Customer;
```



### 원하는 컬럼만 골라 보기

- Customer테이블의 CustomerName, ContactName, Country 컬럼만 보기

```sql
SELECT CustomerName, ContactName, Country
FROM Customers;
```



### 컬럼이 아닌 값 선택하기

- Customer테이블의 CustomerName, 1, NULL, 'Hello' 컬럼만 보기

```sql
SELECT
  CustomerName, 1, 'Hello', NULL
FROM Customers;
```

- 결과

| CustomerName                       | 1    | Hello | NULL |
| :--------------------------------- | :--- | :---- | :--- |
| Alfreds Futterkiste                | 1    | Hello |      |
| Ana Trujillo Emparedados y helados | 1    | Hello |      |
| Antonio Moreno Taquería            | 1    | Hello |      |

존재하지 않는 컬럼은 `SELECT`한 값으로 채워진다. (단, NULL은 NULL로 채워지기 때문에 비어있음)



### 원하는 조건의 row(행)만 걸러서 보기

- Customer테이블의 Country가 'Mexico'인 행만 보기

```sql
SELECT
*
FROM Customers
WHERE Country = 'Mexico';
```

- 결과

| CustomerID | CustomerName                       | ContactName          | Address                       | City        | PostalCode | Country |
| :--------- | :--------------------------------- | :------------------- | :---------------------------- | :---------- | :--------- | :------ |
| 2          | Ana Trujillo Emparedados y helados | Ana Trujillo         | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
| 3          | Antonio Moreno Taquería            | Antonio Moreno       | Mataderos 2312                | México D.F. | 05023      | Mexico  |
| 13         | Centro comercial Moctezuma         | Francisco Chang      | Sierras de Granada 9993       | México D.F. | 05022      | Mexico  |
| 58         | Pericles Comidas clásicas          | Guillermo Fernández  | Calle Dr. Jorge Cash 321      | México D.F. | 05033      | Mexico  |
| 80         | Tortuga Restaurante                | Miguel Angel Paolino | Avda. Azteca 123              | México D.F. | 05033      | Mexico  |



- Orders 테이블의 ShipperID = 1인 행들의 CustomerID, EmployeeID, ShipperID 열 보기

```sql
SELECT CustomerID, EmployeeID, ShipperID
FROM Orders
WHERE ShipperID = 1;
-- WHERE ShipperID < 3; -> 3보다 작은 행들 보기
```

- 결과

| CustomerID | EmployeeID | ShipperID |
| :--------- | ---------- | :-------- |
| 81         | 6          | 1         |
| 84         | 3          | 1         |
| 20         | 1          | 1         |
| 55         | 4          | 1         |
| 7          | 2          | 1         |
| 25         | 4          | 1         |



### 정렬해서 보기

`ASC` : 오름차순 (기본값)

`DESC`: 내림차순

- Customers 테이블에서 ContactName을 기준으로 오름차순하여 모든 열 보기

```sql
SELECT * FROM Customers
ORDER BY ContactName;
```

- 결과

| CustomerID | CustomerName                       | ContactName       | Address                       | City        | PostalCode | Country |
| :--------- | :--------------------------------- | :---------------- | :---------------------------- | :---------- | :--------- | :------ |
| 69         | Romero y tomillo                   | Alejandra Camino  | Gran Vía, 1                   | Madrid      | 28001      | Spain   |
| 52         | Morgenstern Gesundkost             | Alexander Feuer   | Heerstr. 22                   | Leipzig     | 04179      | Germany |
| 2          | Ana Trujillo Emparedados y helados | Ana Trujillo      | Avda. de la Constitución 2222 | México D.F. | 05021      | Mexico  |
| 81         | Tradição Hipermercados             | Anabela Domingues | Av. Inês de Castro, 414       | São Paulo   | 05634-030  | Brazil  |



- OderDetails에서 ProductID를 기준으로 오름차순, 동일한 경우 Quantity를 기준으로 내림차순하여 모든 열 보기

```sql
SELECT * FROM OrderDetails
ORDER BY ProductID ASC, Quantity DESC;
```

- 결과

| OrderDetailID | OrderID | ProductID | Quantity |
| :------------ | :------ | :-------- | :------- |
| 1570          | 10847   | 1         | 80       |
| 1741          | 10918   | 1         | 60       |
| 1271          | 10729   | 1         | 50       |
| 100           | 10285   | 1         | 45       |
| 2022          | 11031   | 1         | 45       |
| 724           | 10522   | 1         | 40       |



### 원하는 만큼만 보기

> **`LIMIT {가져올 갯수}`** 또는 **`LIMIT {건너뛸 갯수}, {가져올 갯수}`** 를 사용하여, 원하는 위치에서 원하는 만큼만 데이터를 가져올 수 있다.

- Customer 테이블에서 모든 컬럼을 포함한 행 10개 보기

```sql
SELECT * FROM Customers
LIMIT 10;
```



- OrderDetails 테이블에서 모든 컬럼을 포함하여 ProductID를 기준으로 오름차순 정렬하여 35번째 행부터 5개 보기

```sql
SELECT * FROM OrderDetails
ORDER BY ProductID
LIMIT 35, 5;
```

- 결과

| OrderDetailID | OrderID | ProductID | Quantity |
| :------------ | :------ | :-------- | :------- |
| 1162          | 10691   | 1         | 30       |
| 1724          | 10911   | 1         | 10       |
| 724           | 10522   | 1         | 40       |
| 30            | 10258   | 2         | 50       |
| 1012          | 10632   | 2         | 30       |





### 얼라이어스(Alias)로 보기

- Customer테이블의 CustomerId행은 '아이디'로, CustomerName행은 '고객명'으로, Address행은 '주소'로 보기

```sql
SELECT
  CustomerId AS '아이디',
  CustomerName AS '고객명',
  Address AS '주소'
FROM Customers;
```

- 결과 

| 아이디 | 고객명                             | 주소                          |
| :----- | :--------------------------------- | :---------------------------- |
| 1      | Alfreds Futterkiste                | Obere Str. 57                 |
| 2      | Ana Trujillo Emparedados y helados | Avda. de la Constitución 2222 |
| 3      | Antonio Moreno Taquería            | Mataderos 2312                |
| 4      | Around the Horn                    | 120 Hanover Sq.               |

