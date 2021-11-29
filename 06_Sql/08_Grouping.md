# Grouping

- GROUP BY는 해당 컬럼의 종류들을 정렬해서 집계한다. (중복 X)



- Customers테이블에서  Country, City, `City, Country`의 컬럼을 Country, City로 집계하여 선택
- Country, City의 조합을 집계한다고 이해하면 됨

```sql
SELECT 
  Country, City,
  CONCAT_WS(', ', City, Country)
FROM Customers
GROUP BY Country, City;
```

| Country   | City         | CONCAT_WS(', ', City, Country) |
| :-------- | :----------- | :----------------------------- |
| Argentina | Buenos Aires | Buenos Aires, Argentina        |
| Austria   | Graz         | Graz, Austria                  |
| Austria   | Salzburg     | Salzburg, Austria              |
| Belgium   | Bruxelles    | Bruxelles, Belgium             |
| Belgium   | Charleroi    | Charleroi, Belgium             |
| Brazil    | Campinas     | Campinas, Brazil               |





### 함수와 활용

- Orders테이블에서 OrderDate마다의 갯수와 OrderDate 행을 OrderDate로 집계

```sql
SELECT
  COUNT(*), OrderDate
FROM Orders
GROUP BY OrderDate;
```

| COUNT(*) | OrderDate  |
| :------- | :--------- |
| 1        | 1996-07-04 |
| 1        | 1996-07-05 |
| 2        | 1996-07-08 |
| 1        | 1996-07-09 |



- OrderDetails테이블에서 ProductID, SUM(Quantity)의 값을 QuantitySum으로 하는 컬럼을 ProductID로 집계한 한 뒤 QuantitySum의 역순으로 정렬

```sql
SELECT
  ProductID,
  SUM(Quantity) AS QuantitySum
FROM OrderDetails
GROUP BY ProductID
ORDER BY QuantitySum DESC;
```

| ProductID | QuantitySum |
| :-------- | :---------- |
| 60        | 1577        |
| 59        | 1496        |
| 31        | 1397        |
| 56        | 1263        |
| 16        | 1158        |
| 75        | 1155        |



- Products 테이블에서  Price의 최댓값, 최솟값, 중간값(소숫점 2자리), 평균(소숫점 2자리)를 구한뒤 CategoryID로 집계한다.

```sql
SELECT
  CategoryID,
  COUNT(*) AS Amount,
  MAX(Price) AS MaxPrice, 
  MIN(Price) AS MinPrice,
  TRUNCATE((MAX(Price) + MIN(Price)) / 2, 2) AS MedianPrice,
  TRUNCATE(AVG(Price), 2) AS AveragePrice
FROM Products
GROUP BY CategoryID; 
```

| CategoryID | Amount | MaxPrice | MinPrice | MedianPrice | AveragePrice |
| :--------- | :----- | :------- | :------- | :---------- | :----------- |
| 1          | 12     | 263.50   | 4.50     | 134.00      | 37.97        |
| 2          | 12     | 43.90    | 10.00    | 26.95       | 23.06        |
| 3          | 13     | 81.00    | 9.20     | 45.10       | 25.16        |
| 4          | 10     | 55.00    | 2.50     | 28.75       | 28.73        |



### ROLLUP

- 전체의 집계값

```sql
SELECT
  Country, COUNT(*)
FROM Suppliers
GROUP BY Country
WITH ROLLUP;
```

|           |          |
| :-------- | :------- |
| Country   | COUNT(*) |
| Australia | 2        |
| Brazil    | 1        |
| Canada    | 2        |
| Denmark   | 1        |
| Finland   | 1        |
|           | 7        |



### HAVING

- 그룹화된 데이터에 조건 걸기

- Suppliers테이블에서 Country로 집계후  Count >= 3인 행에 대해서만 Country, COUNT(*) AS Count컬럼을 선택 

```sql
SELECT
  Country, COUNT(*) AS Count
FROM Suppliers
GROUP BY Country
HAVING Count >= 3;
```

| Country | Count |
| :------ | :---- |
| France  | 3     |
| Germany | 3     |
| USA     | 4     |



Having과 where의 차이는 집계 전 후의 차이

- Orders테이블에서 OrderDate > DATE('1996-12-31')가 참인 테이블을 OrderDate로 집계한 후, Count > 2가 참인 행들의 COUNT(*) AS Count, OrderDate 컬럼 선택

```sql
SELECT
  COUNT(*) AS Count, OrderDate
FROM Orders
WHERE OrderDate > DATE('1996-12-31')
GROUP BY OrderDate
HAVING Count > 2;
```

| Count | OrderDate  |
| :---- | :--------- |
| 3     | 1997-12-16 |
| 3     | 1997-12-18 |
| 3     | 1997-12-22 |
| 3     | 1997-12-24 |
| 3     | 1997-12-26 |





### DISTINCT

- 중복된 값을 제거
- 정렬X