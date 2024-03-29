# 객체지향 쿼리 심화

## 벌크 연산

엔티티를 수정하기 위해서는 영속성 컨텍스트의 변경 감지 기능이나 병합을 사용하고 삭제할때는 em.remove()을 사용함\
하지만 아주 많은 엔티티에 작업을 진행하는건 벌크 연산을 사용할 수 있다\
\


벌크 연산은 em.executeUpdate()이라는 메소드가 사용되고\
이 메소드의 반환 값은 벌크 연산으로 변경사항이 생긴 엔티티 건수를 반환한다\


```java
String q1String = "update Product p set p.price = p.price * 1.1 where p.stockAmount < :stockAmount";

int resultCount = em.createQuery(q1String).setParameter("stockAmount, 10).executeUpdate();
```

이렇게 사용하는데 벌크 연산을 사용하는데 있어서 주의할점이 있다\
벌크 연산은 영속성 컨텍스트를 무시하고 데이터베이스에 직접 쿼리한다는 점을 주의해야 한다\
\


벌크 연산은 직접적으로 데이터베이스에 값을 집어 넣어주고 있기 때문에\
만약 영속성 컨텍스트에 해당 엔티티가 이미 들어가있는 경우에는 값 조회시 연산이 적용되어있지 않은 (영속성 컨텍스트에 저장되있는) 값이 튀어나오는 문제가 있을 수 있다\
**그래서 해결은 어떻게 하냐**\


1. em.refresh()의 사용 : 벌크 연산을 수행한 직후에 em.refresh()로 데이터베이스와 영속성 컨텍스트를 동기화
2. 벌크 연산 먼저 실행 : 가장 실용적인 방법으로, 애초에 조회하기 전에 벌크를 먼저 진행
3. 벌크 연산 수행 후, 영속성 컨텍스트 초기화 : refresh()가 아니라 그냥 벌크 연산과 관련되게 들어오면 em에 있는 엔티티를 깔끔하게 제거하는 것도 방법

\
\
\


## 영속성 컨텍스트와 JPQL

\####쿼리 후 영속 상태인 것과 아닌 것 JPQL의 조회 대상은 엔티티, 임베디드 타입, 값 타입같이 종류가 다양한데,\
JPQL로 엔티티를 조회하면 영속성 컨텍스트에서 관리되지만 엔티티가 아니라면 영속성 컨텍스트에서 관리되지 않는다 = **조회한 엔티티만 영속성 컨텍스트에서 관리됨**\


\####JPQL로 조회한 엔티티와 영속성 컨텍스트 그럼 em에서 엔티티를 조회해서 em에 엔티티가 있는 상황에서 그 값을 조회한다면?\
일단 JPQL로 디비에서 조회한 엔티티가 영속성 컨텍스트에 존재하고 있다면 디비에서 조회한 결과를 버리고 em에 있는 엔티티를 반환함\
현재 방식은

* JPQL로 조회한 엔티티는 영속상태
* 영속성 컨텍스트에 이미 존재하는 엔티티가 있다면 기존 엔티티를 반환

\


근데 왜 디비에서 직접 조회한걸 버리고 굳--이 영속성 컨텍스트에 있는걸 반환할까?\
가능한 방법들을 예상해보자

1. 새로운 엔티티를 영속성 컨텍스트에 하나 더 추가 -> 일단 기본키때문에 에러가 나겠죠?
2. 기존 엔티티를 새로 검색한 엔티티로 대체 -> 나쁘진 않지만 영속성 컨텍스트에서 수정하고 있는 값이 있다면 수정하던 값이 사라지겠지?
3. 기존 엔티티는 그대로 두고 새로 검색한 엔티티를 버린다 -> 그럼 방법이 이것말곤...

\


영속성 컨텍스트는 영속 상태인 엔티티의 동일성을 보장해주기 때문에 em.find()로 조회하든 JPQL으로 조회하든\
사용하는 영속성 컨텍스트가 같으면 동일한 엔티티를 반환\


#### find() vs JPQL

em.find()는 엔티티를 영속성 컨텍스트에서 먼저 찾아보고 없다면 그제야 디비에서 찾는 그런 메소드\
그렇기 때문에 만약 영속성 컨텍스트에 엔티티가 영속되어 있다면 성능상 이점이 있다\
하-지-만\
JPQL은 항상 디비에 직접적으로 SQL을 실행해서 결과를 조회한다\
em.find()는 영속성 컨텍스트를 우선 조회 후 디비를 조회하지만! JPQL은 디비를 먼저 조회하고 영속성 컨텍스트와 비교해서 값이 있으면 디비에서 찾은 값을 버린다\
결과적으로 JPQL의 특징

* JPQL은 항상 디비를 조회
* JPQL로 조회한 엔티티는 영속 상태
* 영속성 컨텍스트에 이미 존재하는 엔티티가 있으면 기존 엔티티를 반환

\
\
\


## JPQL과 플러시 모드

플러시 -> 영속성 컨텍스트의 변경 내역을 디비에 동기화하는 작업\
JPA는 플러시가 일어날 때 영속성 컨텍스트에 등록, 수정, 삭제한 엔티티를 찾아서 SQL을 만들고 디비에 반영\
굳이 플러시 메소드를 호출해서 하는 것도 가능하지만 이외에도 플러시 모드를 설정하는 것을 통해서 모드를 변경하는 것도 가능\


* em.setFlushMode(FlushModeType.AUTO); //커밋 또는 쿼리 실행 시 플러시(기본값)
* em.setFlushMode(FlushModeType.COMMIT); //커밋 시에만 플러시

이렇게 옵션은 성능 최적화를 위해 꼭 필요할때만 사용한다\
\


#### 쿼리와 플러시 모드

JPQL은 영속성 컨텍스트에 있는 데이터를 고려하지 않고 디비에서 데이터를 조회한다\
그렇기 때문에 JPQL을 실행하기 전에 영속성 컨텍스트의 내용을 디비에 반영해야함\
기본적으로 AUTO로 들어가면 알아서 수정된 데이터를 조회할 수 있다는 점이 나쁘지 않다\
그리고 어떻게 보면 auto가 모든 것을 포함해주는 방식인데 왜 commit 모드가 존재하는걸까?\
\


#### 플러시 모드와 최적화

commmit 모드는 트랜잭션을 커밋할 때만 플러시하고 쿼리를 실행할 때는 플러시하지 않음\
그러면 쿼리를 사용하게 되면 영속성 컨텍스트에는 있지만 디비에 적용하지 않은 데이터를 조회할 수 없다\
즉, 그 의미는 결국 디비와 영속성 컨텍스트의 비동기화를 의미하고 이는 데이터의 무결성을 보장해주지 않는 방식이다\
하-지-만, 플러시 모드가 자주 일어나게 되면 그 의미는 즉 성능의 저하를 의미하기 때문에 특별하게 사용하는 경우가 있다\
\
JPA를 사용하지 않고 JDBC를 직접 사용해서 SQL을 실행할 때도 플러시를 고민해봐야함\
JPA를 통하지 않고 JDBC로 쿼리를 직접 실행하면 JPA는 JDBC가 실행한 쿼리를 인식할 방법이 없음\
그래도 특별하게 별도의 JDBC 호출은 플러시를 해도 적용 X -> 알아서 플러시를 통해서 동기화 하라\
\
\


## 마지막 정리

#### JPQL은 SQL을 추상화해서 특정 데이터베이스 기술에 의존하지 않음

#### Criteria, QueryDSL은 JPQL을 만들어주는 빌더의 역할이기 때문에 JPQL에 익숙해져라

#### Criteria, QueryDSL을 사용하게 되면 동적으로 변하는 쿼리를 쉽게 사용할 수 있다

#### Criteria는 JPA에서 공식으로 지원하지만 QueryDSL이 몇배는 더 좋은듯

#### JPA도 네이티브 SQL을 제공하지만 네이티를 사용하면 한가지 디비에 종속적이라 그렇게 좋지는 않음

#### JPQL에서는 한꺼번에 다수의 엔티티에 접근하는 벌크 연산을 지원함

\
\
\
\
\
\
\
\
\
\
