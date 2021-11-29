# 테이블 생성/수정/삭제

- 테이블 만들기

```sql
CREATE TABLE 테이블 이름 (컬럼 이름1 자료형, 컬럼 이름2 자료형, ... )
```



```Sql
CREATE TABLE people (
  person_id INT,
  person_name VARCHAR(10),
  age TINYINT,
  birthday DATE
);
```

people이란 테이블 생성

Column 은 person_id, person_name, age, birthday

- 테이블 변경

```sql
ALTER TABLE people RENAME TO friends, -- 테이블명 변경
CHANGE COLUMN person_id person_id TINYINT, -- 컬럼 자료형 변경
CHANGE COLUMN person_name person_nickname VARCHAR(10),  -- 컬럼명 변경
DROP COLUMN birthday, -- 컬럼 삭제
ADD COLUMN is_married TINYINT AFTER age; -- 컬럼 추가
```

- 테이블 삭제

```sql
DROP TABLE friends;
```



### 데이터 Insert

- 테이블에 데이터를 추가하기

- people 테이블에 (person_id, person_name, age, birthday) 컬럼에 (1, '홍길동', 21, '2000-01-31')값 추가하기

```sql
INSERT INTO people
  (person_id, person_name, age, birthday)
  VALUES (1, '홍길동', 21, '2000-01-31');
  
-- 모든 컬럼에 값 넣을 때는 컬럼명들 생략 가능
INSERT INTO people
  VALUES (1, '홍길동', 21, '2000-01-31');
```

