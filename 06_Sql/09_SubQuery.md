# 서브쿼리

- 쿼리 안에 쿼리 실행



- Categories테이블에서 CategoryID, CategoryName, Description과 ProductName컬럼을 선택
  - ProductName은 Products테이블에서 ProductID = 1인 행의 ProductName컬럼

```sql
SELECT
  CategoryID, CategoryName, Description,
  (
    SELECT 
   		 ProductName 
   	FROM Products
   	WHERE ProductID = 1
  ) AS ProductName
FROM Categories;
```

| CategoryID | CategoryName | Description                                                | ProductName |
| :--------- | :----------- | :--------------------------------------------------------- | :---------- |
| 1          | Beverages    | Soft drinks, coffees, teas, beers, and ales                | Chais       |
| 2          | Condiments   | Sweet and savory sauces, relishes, spreads, and seasonings | Chais       |
| 3          | Confections  | Desserts, candies, and sweet breads                        | Chais       |



- Categories테이블에서 CategoryID컬럼의 값이 [Products테이블에서 Price가 50보다 큰 행들의 CategoryID]에 있는 행의 CategoryID, CategoryName, Description컬럼을 선택

```sql
SELECT
  CategoryID, CategoryName, Description
FROM Categories
WHERE
  CategoryID IN
  (SELECT CategoryID FROM Products
  WHERE Price > 50);
```

| CategoryID | CategoryName   | Description                                 |
| :--------- | :------------- | :------------------------------------------ |
| 1          | Beverages      | Soft drinks, coffees, teas, beers, and ales |
| 3          | Confections    | Desserts, candies, and sweet breads         |
| 4          | Dairy Products | Cheeses                                     |





### 상관 서브쿼리

- Products테이블에서 ProductID, ProductName, CategoryName 컬럼을 선택

  [Categories 테이블에서 C.CategoryID = P.CategoryID 인 행의 CategoryName컬럼을 선택] C == Categories, P == Products

```sql
SELECT
  ProductID, ProductName,
  (
    SELECT CategoryName FROM Categories C
    WHERE C.CategoryID = P.CategoryID
  ) AS CategoryName
FROM Products P;
```

- Product 테이블

| ProductID | ProductName | SupplierID | *CategoryID* | Unit | Price |
| :-------- | :---------- | :--------- | :----------- | :--- | :---- |

- Categories 테이블

| *CategoryID* | CategoryName | Description |
| :----------- | :----------- | :---------- |





| ProductID | ProductName                  | CategoryName |
| :-------- | :--------------------------- | :----------- |
| 1         | Chais                        | Beverages    |
| 2         | Chang                        | Beverages    |
| 3         | Aniseed Syrup                | Condiments   |
| 4         | Chef Anton's Cajun Seasoning | Condiments   |





- CategoryID, CategoryName
- MaximumPrice = [P.CategoryID = C.CategoryID 인 행들의 MAX(Price)]
- AveragePrice = [P.CategoryID = C.CategoryID 인 행들의 AVG(Price)]

- 

```Sql
SELECT
  CategoryID, CategoryName,
  (
    SELECT MAX(Price) FROM Products P
    WHERE P.CategoryID = C.CategoryID
  ) AS MaximumPrice,
  (
    SELECT AVG(Price) FROM Products P
    WHERE P.CategoryID = C.CategoryID
  ) AS AveragePrice
FROM Categories C;
```

| CategoryID | CategoryName | MaximumPrice | AveragePrice |
| :--------- | :----------- | :----------- | :----------- |
| 1          | Beverages    | 263.50       | 37.979167    |
| 2          | Condiments   | 43.90        | 23.062500    |
| 3          | Confections  | 81.00        | 25.160000    |

