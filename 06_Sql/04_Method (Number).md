# 숫자 관련 함수

| 함수                           | 설명                       |
| ------------------------------ | -------------------------- |
| **ROUND**                      | 반올림                     |
| **CEIL**                       | 올림                       |
| **FLOOR**                      | 내림                       |
| **TRUNCATE**(N, n)             | N을 소숫점 n자리까지 선택  |
| **ABS**                        | 절대값                     |
| **GREATEST**                   | (괄호 안에서) 가장 큰 값   |
| **LEAST**                      | (괄호 안에서) 가장 작은 값 |
| **POW**(A, B), **POWER**(A, B) | A를 B만큼 제곱             |
| **SQRT**                       | 제곱근                     |

- Round, Ceil, Floor

```sql
SELECT 
  ROUND(0.5), -- 1
  CEIL(0.4),  -- 1
  FLOOR(0.6); -- 0
```

- Truncate

```sql
SELECT
  TRUNCATE(1234.5678, 1),  -- 1234.5
  TRUNCATE(1234.5678, 2),  -- 1234.56
  TRUNCATE(1234.5678, -1), -- 1230
  TRUNCATE(1234.5678, -2), -- 1200
```

- Abs

```sql
SELECT
	ABS(1),  -- 1
	ABS(-1), -- 1
	ABS(3 - 10); -- 7
```

>- OrderDetails 테이블에서 `Quantity - 10의 절대값이 5보다 작은`[6~14] 인 행의 모든 컬럼 선택
>
>```sql
>SELECT * FROM OrderDetails
>WHERE ABS(Quantity - 10) < 5;
>```

- Greatest, Least

```sql
SELECT 
  GREATEST(1, 2, 3),   -- 3
  LEAST(1, 2, 3, 4, 5); -- 1
```



- Pow, Sqrt

```sql
SELECT
  POW(2, 3),  -- 8
  SQRT(16);   -- 4
```



| 함수      | 설명               |
| --------- | ------------------ |
| **MAX**   | 가장 큰 값         |
| **MIN**   | 가장 작은 값       |
| **COUNT** | 갯수 (NULL값 제외) |
| **SUM**   | 총합               |
| **AVG**   | 평균 값            |

- OrderDetails의 테이블에서 OrderDetailID가 20~30인 행들의 

  최댓값, 최솟값, 개수, 합, 평균 컬럼을 선택

```sql
SELECT
  MAX(Quantity),
  MIN(Quantity),
  COUNT(Quantity),
  SUM(Quantity),
  AVG(Quantity)
FROM OrderDetails
WHERE OrderDetailID BETWEEN 20 AND 30;
```

| MAX(Quantity) | MIN(Quantity) | COUNT(Quantity) | SUM(Quantity) | AVG(Quantity) |
| :------------ | :------------ | :-------------- | :------------ | :------------ |
| 50            | 6             | 11              | 254           | 23.0909       |

