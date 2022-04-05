# 스프링 데이터 JPA 소개
<br><br>

스프링 데이터 JPA는 스프링에서 JPA를 쉽게 사용할 수 있도록 지원해주는 기술이다 <br>
이 기술은 데이터 접근 계층을 만들 때 공통적으로 나오는 crud에 대한 인터페이스를 지원해주고 있다 <br>
또한 레포지토리를 개발할 때 인터페이스만 작성해두면 애플리케이션 실행 시점에 알아서 동적으로 생성해서 주입해준다 <br>
그 의미는 결국 데이터 접근 계층을 개발하는데 있어서 구현 클래스 없이 인터페이스만 작성해도 개발하는 것이 가능하다 <br>
<br>
추가적으로 공통으로 처리할 수 없는 메소드같은 경우에도 스프링 데이터 JPA가 메소드의 이름을 분석해서 JPQL을 실행 <br>
<br><br><br>

## 스프링 데이터 프로젝트
결국 스프링 데이터 JPA는 스프링 데이터 프로젝트의 하위 프로젝트 중 하나이다 <br>
스프링 데이터 프로젝트에는 JPA, 몽고DB, NEO4J, REDIS, HADOOP등 몇 가지에 대한 데이터 저장소에 대한 접근을 추상화해서 개발자 편의를 줄여주었다 <br>
<br><br><br>

## 스프링 데이터 JPA 설정
spring-data-jpa <br>
이 키워드를 통해서 메이븐이든 그레이들 에다가 추가하면 된다 <br>
그리고 스프링 설정에서 JavaConfig을 설정에 <br>
@Configuration, @EnableJpaRepository(basePackages = "jpabook.jpashop.repository") <br>
이 2가지로 환경 설정을 마무리 지을 수 있으며,  <br>
이렇게 해두면 애플리케이션을 실행ㅎ라 때 basePackage에 있는 레포지토리 인터페이스들을 찾아서 <br>
해당 인터페이스를 구현한 클래스를 동적으로 생성하고 스프링 빈으로 등록함을 통해 개발자가 직접 구현 클래스를 만들지 않아도 된다 <br>
<br><br><br>
