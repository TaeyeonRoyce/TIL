# Join



- Categories에 C = Categories, P = Products 이고, C.CategoryID = P.CategoryID를 만족하는 Products를 합친 테이블의 모든 컬럼을 선택

```sql
SELECT * FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID; 
```

| CategoryID | CategoryName | Description                                                | ProductID | ProductName   | SupplierID | CategoryID | Unit                | Price |
| :--------- | :----------- | :--------------------------------------------------------- | :-------- | :------------ | :--------- | :--------- | :------------------ | :---- |
| 1          | Beverages    | Soft drinks, coffees, teas, beers, and ales                | 1         | Chais         | 1          | 1          | 10 boxes x 20 bags  | 18.00 |
| 1          | Beverages    | Soft drinks, coffees, teas, beers, and ales                | 2         | Chang         | 1          | 1          | 24 - 12 oz bottles  | 19.00 |
| 2          | Condiments   | Sweet and savory sauces, relishes, spreads, and seasonings | 3         | Aniseed Syrup | 1          | 2          | 12 - 550 ml bottles | 10.00 |



- Products테이블에서 P = Products, S = Suppliers 이고,  P.SupplierID = S.SupplierID를 만족하는 Suppliers를 합친 테이블에서 Price > 50인 행들을 ProductName으로 정렬한 뒤 [CONCAT( P.ProductName, ' by ', S.SupplierName)]인 Product, S의 Phone, P의 Price의 컬럼을 선택

```sql
SELECT
  CONCAT(
    P.ProductName, ' by ', S.SupplierName
  ) AS Product,
  S.Phone, P.Price
FROM Products P
JOIN Suppliers S
  ON P.SupplierID = S.SupplierID
WHERE Price > 50
ORDER BY ProductName;
```

| Product                                     | Phone           | Price  |
| :------------------------------------------ | :-------------- | :----- |
| Carnarvon Tigers by Pavlova, Ltd.           | (03) 444-2343   | 62.50  |
| Côte de Blaye by Aux joyeux ecclésiastiques | (1) 03.83.00.68 | 263.50 |
| Manjimup Dried Apples by G'day, Mate        | (02) 555-5914   | 53.00  |



- 여러 테이블 합치기
- Categories의 ID와 CategoryName, Products의 ProductName, OrderDetails의 OrderDate, Orders의 Quantity를 합친 테이블

```sql
SELECT 
  C.CategoryID, C.CategoryName, 
  P.ProductName, 
  O.OrderDate,
  D.Quantity
FROM Categories C
JOIN Products P 
  ON C.CategoryID = P.CategoryID
JOIN OrderDetails D
  ON P.ProductID = D.ProductID
JOIN Orders O
  ON O.OrderID = D.OrderID;
```

| CategoryID | CategoryName   | ProductName                   | OrderDate  | Quantity |
| :--------- | :------------- | :---------------------------- | :--------- | :------- |
| 4          | Dairy Products | Queso Cabrales                | 1996-07-04 | 12       |
| 5          | Grains/Cereals | Singaporean Hokkien Fried Mee | 1996-07-04 | 10       |
| 4          | Dairy Products | Mozzarella di Giovanni        | 1996-07-04 | 5        |
| 7          | Produce        | Tofu                          | 1996-07-05 | 9        |

