# R-tree 공간 인덱스 

R-Tree 인덱스 알고리즘을 이용해 2차원의 데이터를 인덱싱하고 검색하는 목적의 인덱스

> A tree data structure used for spatial indexing of multi-dimensional data such as geographical coordinates, rectangles or polygons.
> 지리적 좌표, 직사각형 또는 다각형과 같은 다차원 데이터의 공간 색인에 사용되는 트리 데이터 구조입니다.
>

B-Tree와 내부 매커니즘은 흡사하고, B-Tree는 1차원의 스칼라 값인 반면,

**R-Tree 인덱스는 2차원의 공간 개념값**이다.

MySQL의 공간 확장(Spaital Extension))을 이용하면 간단하게 이러한 기능을 구현할 수 있다.

MySQL의 공간 확장에는 크게 세가지 기능이 포함되어 있습니다.

- 공간 데이터를 저장할 수 있는 데이터 타입

    ```sql
    -- 단일 데이터 타입
    GEOMETRY
    POINT
    LINESTRING
    POLYGON
    
    -- 컬렉션 데이터 타입
    MULTIPOINT
    MULTILINESTRING
    MULTIPOLYGON
    GEOMETRYCOLLECTION
    ```

- 공간 데이터의 검색을 위한 공간 인덱스(R-Tree 알고리즘)

    ```sql
    ST_Contains(g1, g2) -- 포함 여부 검색
    MBRContains(g1, g2) -- MBR을 활용한 전체 포함 여부
    MBRCoveredBy(g1, g2) -- MBR을 활용한 경계선에 포함 여부
    ```

- 공간 데이터의 연산 함수(거리 또는 포함 관계의 처리)

    ```sql
    ST_EndPoint(ls) -- 끝 POINT를 반환
    ST_Distince(g1, g2) -- 두 MBR의 최단거리
    ```


## 구조 및 특성

> GIS 4326은 EPSG 4326이라고도 불리며, 세계적으로 표준화된 좌표 참조 시스템 (CRS, Coordinate Reference System)을 의미합니다.  
  

### __데이터 타입

| 데이터 타입           | 정의                      | SQL 예제                                                                                             |
|------------------|-------------------------|----------------------------------------------------------------------------------------------------|
| Point            | 좌표 공간에서 한 지점의 위치를 표시    | POINT (10 10)                                                                                      |
| LineString       | 다수의 Point를 연결해주는 선분     | LINESTRING (10 10, 20 25, 15 40)                                                                   |
| Polygon          | 다수의 선분들이 연결되어 닫혀 있는 다각형 | POLYGON ((10 10, 10 20, 20 20, 20 10, 10 10))                                                      |
| Multi-Point      | 다수 개의 Point 집합          | MULTIPOINT (10 10, 30 20)                                                                          |
| Multi-LineString | 다수 개의 LineString 집합     | MULTILINESTRING ((10 10, 20 20), (20 15, 30 40))                                                   |
| Multi-Polygon    | 다수 개의 Polygon 집합        | MULTIPOLYGON (((10 10, 15 10, 20 15, 20 25, 15 20, 10 10)), ((40 25, 50 40, 35 35, 25 10, 40 25))) |
| GeomCollection   | 모든 공간 데이터들의 집합          | GEOMETRYCOLLECTION (POINT (10 10), LINESTRING (20 20, 30 40), POINT (30 15))                       |

```sql
SELECT
    name,
    description,
    ST_DISTANCE(
        point, 
        ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326)
    ) AS distance_in_meters
FROM
    locations_mysql
WHERE
    ST_Contains(ST_Buffer(ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326),**644**), point);
```

### __MBR

![https://blog.kakaocdn.net/dn/bw2aRb/btrGfxEmB0h/3kbhgi5oUS4kV4xPrq5W3K/img.png](https://blog.kakaocdn.net/dn/bw2aRb/btrGfxEmB0h/3kbhgi5oUS4kV4xPrq5W3K/img.png)

> MBR ( Minimum Bounding Ractangle )
>

R-Tree의 핵심은 MBR(최소 경계 사각형)입니다.
하나의 도형뿐만 아니라 근처의 도형도 함께 감싸는 저장방식을 통해 도형의 포함 관계를 이용할 수 있습니다.
B-Tree와 유사하게 특정 범위의 도형을 묶어서 저장할 경우, 탐색 시간을 획기적으로 줄일 수 있기 때문입니다.

![https://www.mdpi.com/ijgi/ijgi-05-00119/article_deploy/html/images/ijgi-05-00119-g001.png](https://www.mdpi.com/ijgi/ijgi-05-00119/article_deploy/html/images/ijgi-05-00119-g001.png)

+ 최상위 레벨 : N1, N2  
+ 차상위 레벨 : N3 ,N4 ,N5 ,N6
+ 최하위 레벨 : L7 ~ L16

R-Tree 자료구조는 B-Tree와 흡사한 형태로 구성되어있습니다. 각 노드에 저장할 수 있는 도형의 개수는 사전에 지정되는데요. 최대 M개에서 최소 m(M/2)개 저장할 수 있습니다.
  
또한 B-Tree처럼 리프 노드에 데이터(도형)를 저장하고, 리프 노드가 아닌 노드는 MBR 간의 포함 관계를 표현합니다.

### R-Tree 인덱스의 용도
MySQL에서 R-tree 인덱스는 공간 데이터를 효율적으로 조회하기 위해 사용됩니다. 특히 포함 관계를 다루는 함수인 **_ST_Contains()_** 또는 **_ST_Within()_** 와 같은 함수가 R-tree 인덱스를 효율적으로 사용할 수 있게 합니다.
  
반면에 거리 계산을 수행하는 함수인 **ST_Distance() ** 나 **ST_Distance_Sphere()** 는 R-tree 인덱스를 효율적으로 사용하지 못합니다. 이는 거리 계산이 포함 관계와 달리, 단일 점이나 영역 내의 점들이 아니라 두 점 간의 거리를 기반으로 하기 때문입니다. R-tree 인덱스는 주로 공간적인 포함 관계를 다루도록 설계되어 있으므로, 이러한 거리 계산 함수들은 인덱스의 이점을 충분히 활용하지 못합니다.
  
정리하자면:
+ 효율적으로 사용 가능한 함수: ST_Contains(), ST_Within()
+ 효율적으로 사용되지 않는 함수: ST_Distance(), ST_Distance_Sphere()
  
  
![image_4.png](image_4.png) 

- 효율적으로 사용했을경우 (`type = range`)

![image_5.png](image_5.png)

- 효율적으로 사용하지 못 했을 경우  (`type = ALL`)

### 주의사항 ( 포함 과 거리의 차이 )

> R-Tree 인덱스를 사용하기 위해서 `ST_Contains()`를 사용해야한다.

![https://blog.kakaocdn.net/dn/SqJwg/btrGfVSefAm/iqPbg6QqLtJDLtLYKlcTHK/img.png](https://blog.kakaocdn.net/dn/SqJwg/btrGfVSefAm/iqPbg6QqLtJDLtLYKlcTHK/img.png)

모든 Spaital Data Type은 MBR로 관리가 됩니다.

```sql
CREATE TABLE locations_mysql (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(255),
    point POINT not null SRID 4326,
    description VARCHAR(255)
);

CREATE SPATIAL INDEX idx_point ON locations_mysql(point);

-- 샘플 데이터 삽입
INSERT INTO locations_mysql (name, point, description) VALUES
    ('작심 독서실', ST_GeomFromText('POINT(37.4302406 127.1296837)', 4326), '작심 독서실 6층 입니다.'),
    ('지하철 8호선 모란역', ST_GeomFromText('POINT(37.4333283 127.1289295)', 4326), '출 퇴근 합니다.'),
    ('도미노피자 수진역점', ST_GeomFromText('POINT(37.4367865 127.1407202)', 4326), '도미노 맛있다.'),
    ('잠실 IC', ST_GeomFromText('POINT(37.5192203 127.0955689)', 4326), '한번 찍어봅니다.'),
    ('MBR test', ST_GeomFromText('POINT(37.4396906 127.1385735)', 4326), '반경 1km를 벗어납니다'),
    ('반경 검색 641m', ST_GeomFromText('POINT(37.4355941 127.1330177)', 4326), '구글로 약 641m');
```

> 이제 `ST_Distince()`로 현재 위치와 저장된 데이터의 거리를 구해봅니다.
>

```sql
SELECT
    name,
    description,
    ST_DISTANCE(
        point, 
        ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326) // 현재 좌표
    ) AS distance_in_meters
FROM
    locations_mysql
```

![image_6.png](image_6.png)

> 현재 좌표와 그리고 저장된 데이터의 좌표의 거리를 컬럼으로 확인했습니다.
>

```sql
SELECT
    name,
    description,
    ST_DISTANCE(
        point, 
        ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326)
    ) AS distance_in_meters,
    ST_Contains(ST_Buffer(ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326),642), point) as '642',
    ST_Contains(ST_Buffer(ST_GeomFromText('POINT(37.4303521 127.1299667)', 4326),644), point) as '644'
FROM
    locations_mysql
```

+ 코드 설명  
  ST_GeomFromText =  해당 좌표를 GIS를 4326 으로 지정합니다.
  ST_Buffer 해당 좌표로부터 반지름 644 GIS(4326) 을 기준으로 원을 그립니다.*객체
  ST_Contains 주어진 도형안에 해당 좌표가 있을 경우 1을 반환합니다.

![image_7.png](image_7.png)

> 거리는 641.4 이지만
> MBR을 모두 포함해야 가져올 수 있기때문에 644가 되어야 1이 되는걸 확인할 수 있습니다.
>