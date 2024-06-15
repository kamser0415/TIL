# 멀티 밸류 인덱스(json)

멀티 밸류 인덱스는 하나의 데이터 레코드가 여러 개의 키 값을 가질 수 있는 형태의 인덱스입니다. 이는 특히 JSON 데이터를 처리할 때 유용합니다.  
+ [MySQL 공식문서(Json Values)](https://dev.mysql.com/doc/refman/8.4/en/json-search-functions.html)

### 예시
```sql
-- 테이블 생성
CREATE TABLE tb_multi_value (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    hobbyJSON JSON
);

-- Multi-value index 추가
ALTER TABLE tb_multi_value ADD INDEX idx_hobby ((cast(hobbyJSON->'$.hobby' as unsigned ARRAY)));

-- 데이터 삽입
INSERT INTO tb_multi_value (name, hobbyJSON) VALUES
    ('John', '{"hobby": [20, 30]}'),
    ('Alice', '{"hobby": [10, 22]}'),
    ('Bob', '{"hobby": [10, 30]}');
```

### 데이터 조회
```sql
-- 특정 값이 포함된 레코드 조회
SELECT * FROM tb_multi_value WHERE 10 MEMBER OF (hobbyJSON->'$.hobby');
```  
+ 결과
    ```SQL
    +----+-------+---------------------+
    | id | name  | hobbyJSON           |
    +----+-------+---------------------+
    |  2 | Alice | {"hobby": [10, 22]} |
    |  3 | Bob   | {"hobby": [10, 30]} |
    +----+-------+---------------------+ 
    ```
+ 실행계획
    ```SQL

    +----+-------------+----------------+------+---------------+-----------+-------+
    | id | select_type | table          | type | possible_keys | key       | ref   |
    +----+-------------+----------------+------+---------------+-----------+-------+
    |  1 | SIMPLE      | tb_multi_value | ref  | idx_hobby     | idx_hobby | const |
    +----+-------------+----------------+------+---------------+-----------+-------+ 
    ```

멀티 밸류 인덱스는 VARCHAR/CHAR 타입을 지원하지 않으며 JSON 타입 데이터에 주로 사용됩니다 (MySQL 8.0.32 기준).