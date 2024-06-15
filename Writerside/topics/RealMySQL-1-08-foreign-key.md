다음은 성능 고려 사항을 포함하여 수정된 MySQL 외래키 설명입니다.

---

# 외래키

> 외래키는 InnoDB 스토리지 엔진에서만 생성 가능하며, 자동으로 연관되는 테이블 컬럼에 인덱스까지 생성됩니다.
>
> **외래키가 제거되지 않은 상태에서는 자동으로 생성된 인덱스를 삭제할 수 없습니다.**

## 주요 특징

- **잠금 경합 발생 조건:**
  - 테이블의 변경(쓰기 잠금)이 발생하는 경우에만 잠금 경합(잠금 대기)이 발생합니다.
  - 자식 테이블의 외래 키 컬럼이 변경되면 부모 테이블을 확인해야 하며, 이 상태에서 부모 테이블의 해당 레코드가 쓰기 잠금이 걸려 있으면 데드락이 발생할 수 있습니다.
  - 외래키와 관련되지 않은 컬럼의 변경은 최대한 잠금 경합(잠금 대기)을 발생시키지 않습니다.

- **부모 테이블의 변경 작업 대기:**
  - 자식 테이블이 참조하는 컬럼에 대해 잠금을 획득한 경우, 부모 테이블의 해당 참조 컬럼은 자식 테이블의 수정이 완료될 때까지 데드락 상태가 될 수 있습니다.
  - 데이터베이스에서 외래키를 물리적으로 생성하려면 이러한 잠금 경합까지 고려하여 모델링을 진행하는 것이 좋습니다.

- **읽기 잠금:**
  - 참조키가 부모 테이블에 있는지 확인하는 과정에서 연관된 테이블에 읽기 잠금이 걸립니다.
  - 잠금이 다른 테이블로 확장되면 전체적으로 쿼리의 동시 처리 성능에 영향을 미칩니다.

## 외래키 제약 조건

외래키 제약 조건을 통해 ON DELETE와 ON UPDATE 동작을 설정할 수 있습니다. 이는 부모 테이블의 레코드가 삭제되거나 업데이트될 때 자식 테이블에서 어떤 동작을 할지 정의합니다. (예: CASCADE, SET NULL, RESTRICT 등)

### 예시

#### 테이블 생성 예시

```sql
CREATE TABLE parent (
    id INT PRIMARY KEY,
    name VARCHAR(50)
);

CREATE TABLE child_cascade (
    id INT PRIMARY KEY,
    parent_id INT,
    CONSTRAINT fk_parent
        FOREIGN KEY (parent_id) 
        REFERENCES parent(id)
        ON DELETE CASCADE
        ON UPDATE CASCADE
);
```

위 예시에서는 `child` 테이블이 `parent` 테이블을 참조하는 외래키를 가집니다. 이 외래키에는 `ON DELETE CASCADE`와 `ON UPDATE CASCADE`가 설정되어 있습니다.

- **ON DELETE CASCADE**: 부모 테이블의 레코드가 삭제될 때, 이를 참조하는 자식 테이블의 레코드도 자동으로 삭제됩니다.
- **ON UPDATE CASCADE**: 부모 테이블의 레코드가 업데이트될 때, 이를 참조하는 자식 테이블의 외래키 값도 자동으로 업데이트됩니다.

#### 동작 예시

1. **부모 테이블에 데이터 삽입**

```sql
INSERT INTO parent (id, name) VALUES (1, 'Parent 1');
INSERT INTO child_cascade (id, parent_id) VALUES (1, 1);
```

2. **부모 테이블의 레코드 삭제**

```sql
DELETE FROM parent WHERE id = 1;
```

- 이때 `child` 테이블에서 `parent_id`가 1인 레코드도 자동으로 삭제됩니다.

3. **부모 테이블의 레코드 업데이트**

```sql
UPDATE parent SET id = 2 WHERE id = 1;
```

- 이때 `child_cascade` 테이블에서 `parent_id`가 1인 레코드의 `parent_id` 값도 자동으로 2로 업데이트됩니다.

### 기타 동작

- **SET NULL**: 부모 테이블의 레코드가 삭제되거나 업데이트될 때, 이를 참조하는 자식 테이블의 외래키 값을 `NULL`로 설정합니다.

```sql
CREATE TABLE child_null (
    id INT PRIMARY KEY,
    parent_id INT,
    CONSTRAINT fk_parent_null
        FOREIGN KEY (parent_id) 
        REFERENCES parent(id)
        ON DELETE SET NULL
        ON UPDATE SET NULL
);
```

- **RESTRICT**: 부모 테이블의 레코드가 삭제되거나 업데이트되는 것을 방지합니다. 만약 자식 테이블이 해당 레코드를 참조하고 있다면, 부모 테이블에서 해당 레코드를 삭제하거나 업데이트할 수 없습니다.

```sql
CREATE TABLE child_restrict (
    id INT PRIMARY KEY,
    parent_id INT,
    CONSTRAINT fk_parent_restrict
        FOREIGN KEY (parent_id) 
        REFERENCES parent(id)
        ON DELETE RESTRICT
        ON UPDATE RESTRICT
);
```
## 성능 고려 사항

외래키는 데이터 무결성을 보장하지만, 추가적인 읽기 및 쓰기 작업이 발생하여 성능에 영향을 미칠 수 있습니다. 특히 대용량 데이터를 다루는 경우 주의가 필요합니다.
적절한 인덱스를 생성하고, 외래키 사용 시 쿼리 성능을 주기적으로 모니터링하는 것이 좋습니다.

- **성능 모니터링**: 외래키 사용으로 인한 성능 저하를 방지하기 위해 주기적으로 쿼리 성능을 모니터링하고, 필요한 경우 쿼리를 최적화하거나 인덱스를 재구성합니다.

이와 같이 외래키 제약 조건을 사용하면 데이터 무결성을 보장하면서도 다양한 동작을 정의할 수 있습니다. 이러한 제약 조건을 적절히 활용하여 데이터베이스 모델링을 수행하는 것이 좋습니다.