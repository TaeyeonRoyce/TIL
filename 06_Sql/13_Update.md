# 변경과 삭제

- ### 업데이트 Update

- menus테이블의 menu_id = 12인 행의 menu_name을 '삼선짜장'으로 변경

```sql
UPDATE menus
SET menu_name = '삼선짜장'
WHERE menu_id = 12;
```



- menus테이블의 fk_business_id = 4 이고 menu_name = '국물떡볶이'인 행의 
    menu_name = '열정떡볶이',
    kilocalories = 492.78,
    price = 5000

  로 변경

```Sql
UPDATE menus
SET 
  menu_name = '열정떡볶이',
  kilocalories = 492.78,
  price = 5000
WHERE 
  fk_business_id = 4
  AND menu_name = '국물떡볶이';
```



- menus테이블의 fk_business_id = 8인 행의 price에 1000원 더하여 저장

```sql
UPDATE menus
SET price = price + 1000
WHERE fk_business_id = 8;
```



- menus의 fk_business_id가

  (sections 테이블에서 section_id가 businesses테이블에서  fk_section_id가 같은 행을 합친 테이블에서 section_name 이 '한식'인 행의 business_id 컬럼)

  의 값에 있는 행에 대하여

  menu_name을  CONCAT('전통 ', menu_name) 로 업데이트

```Sql
UPDATE menus
SET menu_name = CONCAT('전통 ', menu_name)
WHERE fk_business_id IN (
  SELECT business_id 
  FROM sections S
  LEFT JOIN businesses B
    ON S.section_id = B.fk_section_id 
  WHERE section_name = '한식'
);
```



- ### 삭제 Drop

- businesses테이블의 status컬럼의 값이 'CLS'인 행 삭제

```sql
DELETE FROM businesses
WHERE status = 'CLS';
```



- DROP의 경우 데이터를 삭제하고 추가해도 AUTOINCREMENT는 유지된다.

3개의 행이 있는 테이블을 모두 지우고 새로운 행을 추가하면,

1개의 행만 존재하지만, 이 행의 ID(AUTOINCREMENT)는 1이 아닌 4가 된다.

- 완전 초기화를 위해선 TRUNCATE를 사용한다

```sql 
TRUNCATE businesses;
```

