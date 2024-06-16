# 클러스터링 인덱스 (MySQL 8.0.32 기준)

## 클러스터링이란?

여러 개를 하나로 묶는다는 의미로 주로 사용됩니다. MySQL 서버에서 클러스터링은 테이블의 레코드를 비슷한 것들(PK 값을 기준)끼리 묶어서 저장하는 형태로 구현되었습니다. 이는 주로 비슷한 값들을 동시에 조회하는 경우가 많다는 점에 착안한 것이며, InnoDB 엔진에서만 지원됩니다.

### 클러스터링 인덱스

클러스터링 인덱스는 테이블의 프라이머리 키(Primary Key)에 대해서만 적용되는 내용이며, 프라이머리 키 값이 비슷한 레코드끼리 묶어서 저장하는 것을 의미합니다.

> 💡 프라이머리 키 값에 의해 레코드의 저장 위치가 결정된다는 것, 즉 프라이머리 키 값이 변경되면 물리적인 저장 위치도 변경되어야 한다는 점입니다.

클러스터링 인덱스는 인덱스 알고리즘이라기보다 테이블 레코드의 저장 방식이라고 볼 수 있어서, 클러스터링 인덱스와 클러스터링 테이블은 동의어로 사용되기도 합니다. 클러스터링 인덱스로 저장되는 테이블은 **프라이머리 키 기반의 검색이 매우 빠르며**, 대신 레코드의 저장이나 프라이머리 키의 변경이 상대적으로 느립니다.

### 프라이머리 키가 변경되었을 때 MyISAM과 InnoDB의 차이

```sql
UPDATE tb_test SET emp_no = 10002 WHERE emp_no = 10007;
```

- MyISAM: 레코드의 주소가 변경되지 않으며, 저장 위치도 변경되지 않습니다.
- InnoDB: 레코드의 주소가 프라이머리 키 값 순서에 따라 디스크 주소로 변경됩니다.

### 세컨더리 인덱스(Secondary Index)에 미치는 영향

- MyISAM: 프라이머리 키가 변경되어도 저장 위치는 변경되지 않기 때문에 프라이머리 키 인덱스와 세컨더리 인덱스(보조 인덱스) 간에 유니크와 NOT NULL 차이만 존재합니다.
- InnoDB: 보조 인덱스 리프 노드 값에 프라이머리 키가 존재하며, 만약 보조 인덱스에 레코드 주소가 들어있다면 프라이머리 키가 변경될 때마다 보조 인덱스의 레코드 주소도 변경되어야 합니다. 이런 오버헤드를 줄이기 위해 보조 인덱스는 프라이머리 키의 값을 저장하도록 구현되었습니다.

### 클러스터링 인덱스의 장점과 단점

**장점**
- 프라이머리 키로 검색할 때 처리 성능이 매우 빠릅니다.
- 테이블의 모든 세컨더리 인덱스가 프라이머리 키를 가지고 있기 때문에 인덱스만으로 처리될 수 있는 경우가 많습니다. 이를 커버링 인덱스라고 하며, 이는 추후 실행 계획에서 다룹니다.

**단점**
- 테이블의 모든 세컨더리 인덱스가 클러스터링 키를 갖기 때문에, 클러스터링 키 값의 크기가 클 경우 전체적으로 인덱스의 크기가 커집니다.
- 세컨더리 인덱스를 통해 검색할 때 프라이머리 키로 다시 한 번 검색해야 하므로 처리 성능이 느립니다.
- INSERT 시 프라이머리 키에 의해 레코드의 저장 위치가 결정되므로 처리 성능이 느립니다.
- 프라이머리 키를 변경할 때 레코드를 DELETE하고 INSERT하는 작업이 필요하므로 처리 성능이 느립니다.

> 💡 OLTP 환경에서 읽기와 쓰기 비율이 8:2 정도이기 때문에 빠르게 읽기가 중요합니다.

## 클러스터링 테이블 사용 시 주의사항

### 클러스터링 인덱스 키의 크기

클러스터링 테이블을 사용할 경우 세컨더리 인덱스는 키 값으로 인덱스 키 값을 포함합니다. 프라이머리 키의 크기가 커지면 세컨더리 인덱스도 자동으로 커집니다. 테이블에 세컨더리 인덱스가 추가될 때마다 프라이머리 키가 커질수록 비례하여 커지게 됩니다.

### 프라이머리 키는 AUTO_INCREMENT보다 업무적인 칼럼으로 생성

프라이머리 키 값에 의해 레코드의 위치가 결정됩니다. 즉, 프라이머리 키로 검색할 경우 (특히 범위로 많은 레코드를 검색하는 경우) 해당 컬럼의 크기가 크더라도 업무적으로 해당 레코드를 대표할 수 있다면 그 칼럼을 키로 설정하는 것이 좋습니다.

### 프라이머리 키는 반드시 명시할 것

InnoDB 테이블에서 프라이머리 키를 정의하지 않으면 내부적으로 일련번호 칼럼을 추가합니다. 이 칼럼은 사용자에게 보여지지 않기 때문에 접근할 수 없으며, ROW 기반 복제나 InnoDB 클러스터에서는 모든 테이블이 프라이머리 키를 가져야만 정상적인 복제 성능을 보장할 수 있습니다. 따라서 꼭 명시적으로 생성해야 합니다.

### 인조 식별자로 사용할 경우

복합 기본키로 사용할 경우 보조 인덱스가 필요 없다면 상관없지만, 보조 인덱스도 필요하고 프라이머리 키의 크기도 길다면 프라이머리 키를 대체하기 위해 인위적으로 AUTO_INCREMENT를 사용할 수 있습니다. 조회보다 INSERT 위주의 테이블은 인조 식별자가 성능 향상에 도움이 됩니다.