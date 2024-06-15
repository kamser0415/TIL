# 전문 검색 인덱스(FullText Index)

> B-Tree 인덱스는 실제 컬럼값이 1MB라도 전체 키 값을 1MB를 사용하는 것이 아닌 앞글자부터 1,000 byte(MyISAM), 3072 byte(InnoDB) 까지만 잘라서 인덱스 키로 사용합니다. 이러한 특성 때문에 전체 일치 또는 좌측 일부 일치와 같은 검색만 가능합니다.
> 문서의 내용 전체를 인덱스화하여 특정 키워드를 문서에서 검색하는 전문(Full Text) 검색에는 InnoDB나 MyISAM 스토리지 엔진에서 제공하는 일반적인 용도의 B-Tree를 사용할 수 없습니다.

💡 문서 내용 전체를 인덱스화하여 특정 키워드가 포함된 문서를 검색하는 이러한 인덱싱 알고리즘을 전문 검색(FullText Index) 인덱스라고 합니다. 이는 특정 알고리즘을 지칭하는 것은 아닙니다.

### 전문 검색 인덱스 알고리즘

> 사용자가 검색하게 될 키워드를 분석해 내고, 빠른 검색용으로 사용할 수 있게 키워드로 인덱스를 구축하는 알고리즘을 말합니다.

문서의 키워드를 인덱싱하는 기법으로 구분합니다.

1. **어근 분석**
2. **n-gram 분석 알고리즘**

## 어근 분석 알고리즘

어근 분석 알고리즘은 두 가지 중요한 과정을 거쳐서 색인 작업이 수행됩니다.

1. 불용어(Stop Word) 처리
2. 어근 분석(Stemming)

### 불용어 처리란?
검색에서 별 가치가 없는 단어를 모두 필터링해서 제거하는 작업을 의미합니다.

> 불용어의 개수는 많지 않기 때문에 모두 상수로 정의해서 사용하는 경우가 많고,
> MySQL 서버는 불용어가 소스코드에 정의돼 있지만, 이를 무시하고 사용자가 별도로 불용어를 정의할 수 있는 기능도 제공합니다.

### 어근 분석이란?
어근 분석은 검색어로 선정된 단어의 원형을 찾는 작업을 말합니다.

영어와 다르게 한글, 일본어의 경우 단어의 변형 자체는 거의 없기 때문에 어근 분석보다 문장의 형태소를 분석해서 명사와 조사를 구분하는 기능이 더 중요시됩니다.

> 형태소 분석 라이브러리(MeCab)를 주로 사용합니다.
> 설치 후 단어 사전이 필요하고, 문장을 해체하여 각 단어의 품사를 식별할 수 있는 문장의 구조 인식이 필요합니다. 한글에 맞게 완성도를 갖추는 작업은 시간과 노력이 필요합니다.

## n-gram 분석 알고리즘

MeCab을 위한 형태소 분석은 매우 전문적인 전문 검색 알고리즘이어서, 일반적인 검색 엔진을 고려하는 것이 아니라면 범용적으로 적용하기 어렵습니다.

이런 단점을 보완하기 위한 방법으로 n-gram 알고리즘이 도입되었습니다.

n-gram은 단순히 키워드를 검색해내기 위한 인덱싱 알고리즘이라고 할 수 있습니다.

> n-gram이란 본문을 무조건 몇 글자씩 잘라서 인덱싱하는 방법입니다.
> 형태소 분석보다는 알고리즘이 단순하고, 국가별 언어에 대한 이해와 준비 작업이 필요 없지만 만들어진 인덱스의 크기는 상당히 큰 편입니다.

## 인덱스 크기 차이
  
전문 검색 인덱스를 적용할 테이블을 생성합니다.
```sql
CREATE TABLE articles (
       id INT UNSIGNED AUTO_INCREMENT NOT NULL PRIMARY KEY,
       title VARCHAR(200),
       body TEXT,
       FULLTEXT (title, body) -- with parser ngram
) ENGINE=InnoDB;
     
INSERT INTO articles (title, body) VALUES
   ('MySQL Tutorial', 'DBMS stands for DataBase ...'),
   ('How To Use MySQL Well', 'After you went through a ...'),
   ('Optimizing MySQL', 'In this tutorial we will show ...'),
   ('1001 MySQL Tricks', '1. Never run mysqld as root 2. ...1001'),
   ('MySQL vs. YourSQL', 'In the following database comparison ...'),
   ('MySQL Security', 'When configured properly, MySQL ...'); 
```  
### 생성된 인덱스 항목 확인 방법
```SQL
-- 'database_name/table_name';
SET GLOBAL innodb_ft_aux_table = 'employees/articles';
SELECT count(*) FROM INFORMATION_SCHEMA.INNODB_FT_INDEX_CACHE;
```  
이 두 명령어를 같이 사용하면 특정 테이블의 전체 텍스트 인덱스 캐시의 상태를 확인할 수 있습니다. 
이를 통해 해당 테이블의 텍스트 인덱스의 크기나 캐시 사용량을 점검할 수 있습니다.

+ 어근 분석 인덱스 크기
    ```SQL
    SET GLOBAL innodb_ft_aux_table = 'employees/articles';
    SELECT count(*) FROM INFORMATION_SCHEMA.INNODB_FT_INDEX_CACHE;
    +----------+
    | count(*) |
    +----------+
    |       34 |
    +----------+
    ```

+ ngram 인덱스 크기
    ```SQL
    SET GLOBAL innodb_ft_aux_table = 'employees/articles_ngram';
    SELECT count(*) FROM INFORMATION_SCHEMA.INNODB_FT_INDEX_CACHE;
    +----------+
    | count(*) |
    +----------+
    |      132 |
    +----------+
    ```
### 인덱스 크기 정리
어근 분석일 경우 34개, n-gram (default = 2) 132개의 차이가 발생합니다.

```sql
SELECT 
    index_name,
    ROUND(stat_value * @@innodb_page_size / 1024 / 1024, 2) AS size_in_mb
FROM mysql.innodb_index_stats
WHERE database_name = 'study_db'
AND stat_name = 'size'
AND table_name = 'articles';
```

+ 좀 더 정확한 인덱스 크기는 위 SQL로 실행하면 확인할 수 있습니다. 다만 데이터가 작을 경우 인덱스 페이지는 하나에 관리되어 차이가 없을 수 있습니다.

## 텍스트 분석 방식의 차이
+ **MySQL Tutorial'**, **'DBMS stands for DataBase ...'**

+ 어근 분석 방식
  + `database dbms mysql stands tutorial`
  + 중복단어 제거, 불용어를 제거한걸 확인할 수 있습니다.

+ n-gram (Default = 2) 방식 
  + 띄어쓰기(공백)와 마침표(.)를 기준으로 단어를 구분하고, 2글자씩 중첩해서 토큰으로 분리됩니다.
  + `Database` ⇒ da at ta ab ba as se 로 나뉘고 중복은 제거됩니다.
  + `bm db ds fo ms my nd ql se sq st tu ut ys`

## 불용어 변경 및 삭제
불용어는 가치 없는 단어를 필터링하는 역할이지만, `this`는 불용어로 저장되어 있어서 th hi is가 들어간 토큰들은 전부 제외됩니다. 
불용어를 완전히 무시하거나, 직접 불용어를 등록하는 방법을 권장합니다.

### 불용어 무시 처리
my.cnf의 ft_stopword_file 시스템 변수를 빈 문자열로 설정합니다.   
MySQL 서버가 시작될 때만 인지하기 때문에 설정 변경 후 재시작해야 합니다.

+ `ft_stopword_file=''` 기본 설정 제거 
+ `ft_stopword_file = "경로/불용어파일.txt"` 커스텀 파일 지정
  + 예시: `ft_stopword_file = "/data/my_custom_stopword.txt"`
    > `.txt` 파일에는 불용어를 한 줄씩 입력하면 됩니다.

### 불용어 InnoDB 스토리지 엔진을 사용한 테이블에 적용
```sql
CREATE TABLE custom_stopwords (
    id INT AUTO_INCREMENT PRIMARY KEY,
    word VARCHAR(255) NOT NULL
);
INSERT INTO custom_stopwords (word) VALUES
    ('a'),
    ('an'),
    ('the');
```
테이블에 불용어로 등록할 문자를 저장합니다. 
이후 전체 불용어 테이블로 `database/table_name`으로 설정합니다.
```SQL
SET GLOBAL innodb_ft_server_stopword_table = 'study_db/custom_stopwords';
-- 또는
innodb_ft_server_stopword_table = study_db/custom_stopwords; -- 재시작 필요
```

## 전문 검색 인덱스 가용성(정성 처리할 수 있는 능력)

- 쿼리 문장이 전문 검색을 위한 문법 **`(MATCH … AGAINST …)`** 을 사용
- 테이블이 전문 검색 대상 컬럼에 대해서 **전문 인덱스를 보유해야합니다.**

+ 전문 검색 문법을 사용하지 않을 경우:
    ```sql
    EXPLAIN SELECT * FROM articles WHERE body LIKE CONCAT('%', 'DBMS', '%');  
    
    +----+-------------+----------+------+------+---------+------+----------+-------------+
    | id | select_type | table    | type | key  | key_len | rows | filtered | Extra       |
    +----+-------------+----------+------+------+---------+------+----------+-------------+
    |  1 | SIMPLE      | articles | ALL  | NULL | NULL    |    6 |    16.67 | Using where |
    +----+-------------+----------+------+------+---------+------+----------+-------------+
    ```

+ 전문 검색 문법을 사용한 경우:
    ```sql
    EXPLAIN SELECT * FROM articles WHERE MATCH (title, body) AGAINST ('DBMS' IN BOOLEAN MODE);
    
    +----+-------------+----------+----------+---------------+-------+---------+-------+------+----------+-----------------------------------+
    | id | select_type | table    | type     | possible_keys | key   | key_len | ref   | rows | filtered | Extra                             |
    +----+-------------+----------+----------+---------------+-------+---------+-------+------+----------+-----------------------------------+
    |  1 | SIMPLE      | articles | fulltext | title         | title | 0       | const |    1 |   100.00 | Using where; Ft_hints: no_ranking |
    +----+-------------+----------+----------+---------------+-------+---------+-------+------+----------+-----------------------------------+
    ```