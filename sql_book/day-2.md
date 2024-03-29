# Day 2

_다양한 데이터베이스 종류에 대해서 알아보자_

DBMS에는 여러가지 종류가 존재한다. 항상 다양한 서비스를 만족시키는 데이터베이스들이 튀어나오다 보니까 정말 다양하게 존재하고 있는데 역시 가장 많이 사용하기도 하고 SQL이라는 언어를 통해서 데이터를 다루는 데이터베이스를 관계형 데이터베이스(RDB-Relational DataBase)이라고 한다.





## 데이터베이스의 종류

오래된 순서대로이다.

### 계층형 데이터베이스

역사가 오래된 데이터베이스로, 폴더와 파일등을 계층 구조로 데이터를 저장하는 방식

하드디스크와 같은 곳에서 사용하고 있고 서비스 데이터를 저장하기에는 채택하기 어려움

### 관계형 데이터베이스

관계 대수를보고 만든 데이터베이스이기 때문에 관계형 데이터베이스라고 호칭한다.

관계대수란, 행과 열을 가지는 **표 형식 데이터**를 저장하는 형태의 데이터베이스를 의미한다.

표 형식 = 2차원 데이터로 가로 = 열, 세로 = 행 이러한 형태로 데이터를 저장하고 있는 방식이다.

### 객체지향 데이터베이스

우리가 알고 있는 객체 지향 언어로는 C++, java 등이 존재하는데, 데이터베이스의 구성 또한 객체를 중심으로 해서 프로그래밍할 수 있는 객체지향 형태의 데이터베이스이다.

### XML 데이터베이스

XML은 자료형식으로, <태그>\</태그> 이러한 형태의 태그 방식으로 구성된 언어를 의미하는데, 이러한 형식으로 저장하는 데이터베이스를 호칭하고, 이 데이터베이스는 특별하게 SQL명령어를 사용하지 않고 XQuery라는 전용 명령어를 사용한다는 특징이 있다.

### key-value 스토어(KVS)

키와 그 키에 해당하는 값이라는 형태로 데이터를 저장하는 데이터베이스이다. 이 데이터베이스는 최근에 NoSQL(Not Only SQL)라는 슬로건으로 부터 생겨난 데이터베이스로 MongoDB와 같은 데이터베이스가 존재한다.

**이렇게 다양한 종류의 데이터베이스들이 존재하지만 SQL을 사용하는 것은 RDBMS, 관계형 데이터베이스이기 때문에 주로 관계형 데이터베이스를 다룰 예정이다**





## RDBMS 사용 시스템

옛날부터는 메인프레임(대용 범용기기)를 통해서 데이터베이스에 접근하곤 했지만 최근에는 메인 프레임이 단순하게 작은 워크스테이션으로 대체가 되었다. 그 소형 워크스테이션의 서버로 RDBMS가 사용되기 시작했고 그렇게 서버와 클라이언트의 구조로 잡혀가기 시작했다.

가장 많이 사용되는 인프라는 인터넷이다. 최근에는 뭐 안드로이드 핸드폰에도 SQlite라고 내장 데이터베이스도 생겨난다..!





## 데이터베이스 제품

RDBMS에도 정—말 다양한 종류가 존재한다.

### Oracle

오라클에서 개발한 RDBMS로 가장 많이 사용되는 데이터베이스이고 거의 RDBMS의 표준

### DB2

IBM에서 개발한 RDBMS로 이것도 오래되었지만 이 데이터베이스는 IBM에서만 사용할 수 있게 처음 만들었기 때문에 시장 점유율에서 오라클에게 밀린거 같음

### SQL Server

마이크로소프트에서 개발한 RDBMS로 윈도우에서만 동작하는 데이터베이스이지만 현재 윈도우의 사용자가 매우 많기 때문에 사용자가 넓어 지고 있다.

### PostgreSQL

오픈 소스 커뮤니티에서 개발한 RDBMS이다. 때문에 무료이기 때문에 사용하기 편하지만 이 데이터베이스의 기반은 대학교에서 만들어진 데이터베이스이기 때문에 매우 실험적인 기능들도 있고 구조 또한 독특함

### MySQL

이것 또한 오픈 소스 커뮤니티에서 개발한 RDBMS이다. 처음에는 ‘경량'데이터베이스를 강조하고 출시되었지만 지금은 훌륭한 데이터베이스 중 하나임

### SQLite

폰에서 사용한다고 위에서 언급한 것 처럼 임베디드 시스템에 자주 사용되는 작은 RDBMS





## SQL의 방언과 표준화

기본적으로 RDBMS을 조작하는 언어는 SQL이다. 하지만 정말 다양한 데이터베이스들이 각각의 고유한 특징을 가지다 보니까 각 데이터베이스에 맞는 특별한 명령어가 필요하게 되었고 그렇게 다양한 **고유 방언**이 생겼다.

예시로는 키워드 생략이 존재한다.

데이터를 삭제하는데 있어서 DELETE 컬럼 FROM 테이블 WHERE=’조건' 이런 방식으로 데이터를 삭제하는 것이 가능한데,

Oracle이나 SQL Server에서는 FROM을 생략해도 문제가 되지 않는다.

하지만 DB2, PostgreSQL, MySQL에서는 FROM을 생략하면 구문에러가 발생한다.

이외에도 다양한 방언들이 존재하고 있는데, 방언은 방언일 뿐

**표준 SQL을 잘 익히는게 중요하다**









