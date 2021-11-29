# 테이블 관리

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

```sql
-- 일부 컬럼에만 값 넣기 가능 (NOT NULL은 생략 불가)
INSERT INTO people
  (person_id, person_name, birthday)
  VALUES (3, '임꺽정', '1995-11-04');
```

```sql
-- 여러 행을 한 번에 입력 가능
INSERT INTO people
  (person_id, person_name, age, birthday)
  VALUES 
    (4, '존 스미스', 30, '1991-03-01'),
    (5, '루피 D. 몽키', 15, '2006-12-07'),
    (6, '황비홍', 24, '1997-10-30');
```



### 데이터 제약

| 제약               | 설명                               |
| ------------------ | ---------------------------------- |
| **AUTO_INCREMENT** | 새 행 생성시마다 자동으로 1씩 증가 |
| **PRIMARY KEY**    | 중복 입력 불가, NULL(빈 값) 불가   |
| **UNIQUE**         | 중복 입력 불가                     |
| **NOT NULL**       | NULL(빈 값) 입력 불가              |
| **UNSIGNED**       | (숫자일시) 양수만 가능             |
| **DEFAULT**        | 값 입력이 없을 시 기본값           |

```sql
CREATE TABLE people (
  person_id INT AUTO_INCREMENT PRIMARY KEY,
  -- person_id는 int형이고, 새 행 생성시마다 1씩 증가되며 중복입력이 불가하다
  
  person_name VARCHAR(10) NOT NULL,
  -- person_name은 최대 10자리의 문자이고 NULL은 불가하다
  
  nickname VARCHAR(10) UNIQUE NOT NULL,
  -- nickname은 최대 10자리의 문자이고 중복 입력과 NULL이 불가하다
  
  age TINYINT UNSIGNED,
  -- age는 tinyint형이고 양수만 가능하다
  
  is_married TINYINT DEFAULT 0
  -- is_married는 tinyint형이고 NULL값이면 0이 들어간다
);
```



####  **PRIMARY KEY** (기본키)

- 테이블마다 하나만 가능
- 기본적으로 인덱스 생성 (기본키 행 기준으로 빠른 검색 가능)
- 보통 **AUTO_INCREMENT**와 함께 사용
- 각 행을 고유하게 식별 가능 - 테이블마다 하나씩 둘 것