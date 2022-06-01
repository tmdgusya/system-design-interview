## 데이터베이스

### 어떤 관계형 데이터베이스를 사용할 것인가?

전통적으로 많이 쓰이는 relational database 와 non-relational database 사이에서 보통 선택할 수 있다.
관계형 데이터 베이스의 경우 데이터를 테이블 형태로 표현하며 테이블관의 관계를 표현할 수 있다. 따라서 내부적으로 `join` 함수를 지원한다.

비관계형 데이터베이스는, CouchDB, Cassandra 등등이 존재한다. NoSQL DB 는 보통 네가지 부류로 나눌수 있는데 **key-value store / graph store, column store, document store** 가 그 종류들이다.
비관계형 데이터베이스는 보통 join 을 지원하지 않는다. 사실 join 을 지원한다고 해도 내부적으로 성능이 좋지 않은걸로 알고 있다. 필자가 설명하는 비관계형 데이터 베이스를 선택하는 기준은 아래와 같았다.

- 아주 낮은 Latency 요구
- 다루는 데이터가 unstructured 함
- 데이터를 직렬화하거나 역직렬화 할 수 있기만 하면 됨
- 아주 많은 데이터를 저장해야 함

나는 위의 옵션들이 상당히 맘에 들고 잘 설명하고 있다는 생각이 들었다.

### 수직적 규모 확장 vs 수평적 규모 확장

#### 수직적 규모 확장
    - 단순하고 가장 하기 쉬움
    - Server 에 CPU 나 메모리를 무한대로 증설할 방법은 없음
    - FailOver 나 다중와 방안을 제시하지 않음.

#### 수평적 규모 확장
    - FailOver 와 다중화의 방안 제시
    - 수평 확장 되었을때 어떻게 부하를 분산하는지 방법 제시가 필요함 -> 로드밸런서

### 데이터베이스 다중화

#### slave-master 

write only master / almost situtation read from slave

#### 장점

- performance : insert / update query is goes to only master store and select query is goes to slave store
- reliability : if master db is downed, but we'll find data in slave db
- availability

### Cache

Cache is it in memory that expensive computed result and prequently referenced data. So, after request faster than procceed before request

#### Cache Strategy

- When is proper using cache store?
    - Is prequently data updated? no
    - Is prequently data reference from someone? yes

- what is stored in cache?
    - Is need persistence? no
    - It doesn't matter if your data completely deleted? yes

- When expired data in cache store?
    - Did you implemented how to invalid data? if you said noop then you should implement that

- How maintian consistency data that each same between cache store and real-data store?

- How handle exception in your storage?
    - Is store that spof?

- How to mount that size of cache?