---
description: java8에서부터 제공해주는 유용한 기능들
---

# Stream, Optional, 람다식

### Functional Programming(함수형 프로그래밍)?

대입문을 사용하지 않은 프로그래밍

부수효과가 없는 순수 함수를 1급 객체로 간주하여 인자로 넘기거나 반환 값으로 사용할 수 있다.

* 부수효과 : 변수의 값이 변경 or 객체의 필드 값을 변경함 or 예외나 오류가 발생해서 실행이 중단 or 콘솔 또는 파일 IO가 발생
* 1급 객체
  * 변수나 데이터 구조 안에 담을 수 있다.
  * 인자로 전달할 수 있다.
  * 반환 값으로 사용할 수 있다.
  * 할당에 이용된 이름과 무관하게 고유한 구별이 가능하다.

###

### 람다식?

함수를 하나의 식으로 표현한 것이다. 메소드의 이름을 따로 지정해서 사용하지 않기 때문에 익명 함수의 한 종류이다.

익명함수들은 모두 1급 객체이고 1급 객체는 변수처럼 사용 가능하다는 것이 특징이다.

람다 방식은 메소드 명이 필요없으며, 괄호와 화살표를 이용해서 함수를 선언한다.

람다 방식을 사용하면 불필요한 코드를 줄일 수 있고, 가독성을 높이는 것이 가능하다.

코드를 간결하게 만들어서 가독성이 높고 병렬 프로그래밍이 용이하다.

하지만, 재사용이 불가능하고, 디버깅이 어렵고 재귀로 만들 경우에는 부적합하다.

###

### Functional Interface(함수형 인터페이스)?

참고 : [https://mangkyu.tistory.com/113](https://mangkyu.tistory.com/113)

람다식 와 같은 순수 함수와 일반 함수를 구분하기 위해서 함수형 인터페이스가 등장

* 순수 함수 : 동일한 입력을 받았을 때, 항상 동일한 출력을 해야 하는 함수

functional interface란 함수를 1급 객체처럼 다룰 수 있게 해주는 애노테이션(@FunctionalInterface)으로, 인터페이스에 선언해서 단 하나의 추상 메소드만을 갖도록 제한한다.

이미 정의되어있는 함수형 인터페이스

* Supplier\<T> : 매개변수 없이 반환 값만을 갖는 함수형 인터페이스, T get()을 추상 메소드로 가지고 있다.
  * **input값이 없기 떄문에 내부에서 랜덤함수를 사용하는게 아니라면 항상 같은 값을 리턴해주는 특징!!!**
* Consumer\<T> : 객체 T를 매개변수로 받아서 사용하고, 반환 값은 없는 함수형 인터페이스이다.
  * **인자를 받아서 소모한다는 의미**
  * 추상 메소드로 accept와 andThen을 가지고 있다.
  * void accept(T t)를 호출하면서 인자를 전달하게 되면, 구현된 내용이 수행된다.
  * andThen이라는 함수를 사용해서 다음 Consumer를 연쇄적으로 사용 가능 → 원하는 순서대로 andThen을 붙혀주고, accept에 계산하려는 값 넣어줌!
* Function\<T, R> : 객체 T를 매개변수로 받아서 처리하고 R로 반환하는 함수형 인터페이스이다.
  * **전형적인 함수를 지원한다 = 하나의 인자와 리턴타입을 가지고 있다.(제네릭으로 지정가능)**
  * R apply(T t)를 추상 메소드로 가지고 있다. Consumer와 동일하게 andThen 이라는 함수를 지원하고, compose이라는 함수를 사용해서 첫 번째 함수 실행 이전에 먼저 함수를 실행할 수 있다(andThen과 반대), 그리고 identity 라는 static 함수를 사용해서 자기 자신을 반환할 수 있다.
  * compose를 사용하게 되면 인자값으로 준 함수를 먼저 수행하고, 그리고 처음의 값을 먼저 수행한다.
* Predicate\<T> : 객체 T를 매개변수로 받아 처리한 후, Boolean을 반환한다.
  * **Function과 비슷하지만 리턴 타입이 Boolean으로 고정**
  * test()함수를 사용해서 안에 인자 값에 대한 함수 계산이 true인지 false인지 확인
  * and()함수를 통해서 연결해서 사용 - 논리연산의 and와 같음
  * or()함수를 통해서 연결해서 사용 - 논리연산의 or와 같음
  * isEqual()

\<? super 클래스>

매개변수의 자료형을 특정 클래스와 그 클래스의 상위 클래스로만 제한함

\<?> 은 어떤 자료형도 매개변수로 받을 수 있는 와일드카드이다.

메소드 참조 : 함수형 인터페이스를 람다식이 아닌 일반 메소드를 참조시켜서 선언하는 방법

3가지 조건을 만족해야한다.

1. 함수형 인터페이스의 매개변수 타입 = 메소드의 매개변수 타입
2. 함수형 인터페이스의 매개변수 개수 = 메소드의 매개변수 개수
3. 함수형 인터페이스의 반환형 = 메소드의 반환형

이렇게 참조 가능한 메소드는 클래스이름::메소드이름 이렇게 사용할 수 있다.

###

### Stream? Optional?

why? - 기존에 길었던 코드들을 짧게 사용할 수 있도록 보완하고자 나온 기능들이다.

###

### Stream?

Stream API는 데이터를 추상화해서 다루기 때문에 다양한 방식으로 데이터를 읽고 쓰기 위한 공통된 방법을 제공한다.

그렇기 때문에 Stream API를 이용하면 배열이나 Collection뿐만 아니라 파일에 저장된 데이터도 모두 같은 방법으로 다룰 수 있게 된다 - Collection에서 Stream기능을 지원!

데이터를 모아서 원하는 방식으로 처리할 때 사용한다. - DB관련해서 사용할 때 많이 사용된다.

Stream은 데이터를 읽어들여서 처리만 할 뿐, 소스를 변경하지 않는 것이 장점이다.

Stream은 작업을 내부 반복하기 때문에 간결하게 할 수 있다.

Stream API은 이러한 데이터 소스에서 사용할 수 있다.

1. 컬렉션
2. 배열
3. 가변 매개변수
4. 지정된 범위의 연속된 정수
5. 특정 타입의 난수들
6. 람다 표현식
7. 파일
8. 빈 스트림

Stream 의 기본 타입

* IntStream
* LongStream
* DoubleStream

Stream 중계 연산과 그 메소드

* filter : 내부에서 메소드를 사용해서 원하는 조건만 찾는 메소드
* map : 모든 값들에 대해서 메소드를 적용시켜서 결과값으로 바꿔주는 메소드
  * mapTo자료형 : 원하는 자료형으로 변경해주는 메소드
* sorted : comparator를 사용해서 스트림을 정렬해주는 메소드
* forEach : list가 들어왔을때 list의 모든 값들에 대해서 원하는 작업을 해주는 메소드
* ifPresent : 값이 만약 존재한다면 원하는 작업을 해주는 메소드

```java
List<OnlineClass> springClasses = new ArrayList<>();
        springClasses.add(new OnlineClass(1, "spring boot", true));
        springClasses.add(new OnlineClass(2, "spring data jpa", true));
        springClasses.add(new OnlineClass(3, "spring mvc", false));
        springClasses.add(new OnlineClass(4, "spring core", false));
        springClasses.add(new OnlineClass(5, "rest api development", false));

        System.out.println("==============================================");
        System.out.println("spring으로 시작하는 수업 ");
        springClasses.stream()
                .filter(c -> c.getTitle().startsWith("spring"))
                    .forEach(onlineClass -> System.out.println(onlineClass.getTitle()));
        System.out.println("==============================================");

        System.out.println("==============================================");
        System.out.println("close 되지 않은 수업 ");
        springClasses.stream()
                        .filter(Predicate.not(OnlineClass::isClosed))
                                .forEach(onlineClass -> System.out.println(onlineClass.getTitle()));
        System.out.println("==============================================");

        System.out.println("==============================================");
        System.out.println("수업 이름만 모아서 스트림 만들기 ");
        springClasses.stream()
                        .map(OnlineClass::getTitle)
                                .forEach(System.out::println);
        System.out.println("==============================================");


        List<OnlineClass> javaClasses = new ArrayList<>();
        javaClasses.add(new OnlineClass(6, "The Java test", true));
        javaClasses.add(new OnlineClass(7, "The Java, Code manipulation", true));
        javaClasses.add(new OnlineClass(6, "The Java, 8 to 11", false));

        List<List<OnlineClass>> keesunEvents = new ArrayList<>();
        keesunEvents.add(springClasses);
        keesunEvents.add(javaClasses);

        System.out.println("==============================================");
        System.out.println("두 수업 목록에 들어있는 모든 수업 아이디 출력");
        keesunEvents.stream()
                .flatMap(Collection::stream)
                        .forEach(oc -> System.out.println(oc.getId()));
        System.out.println("==============================================");

        System.out.println("==============================================");
        System.out.println("10부터 1씩 증가하는 무제한 스트림 중에서 앞에 10개 빼고 최대 10개 까지만");
        Stream.iterate(10, i -> i+1)
                        .skip(10)
                                .limit(10)
                                        .forEach(System.out::println);
        System.out.println("==============================================");

        System.out.println("==============================================");
        System.out.println("자바 수업 중에 Test가 들어있는 수업이 있는지 확인 ");
        System.out.println(javaClasses.stream().anyMatch(oc -> oc.getTitle().contains("Test")));
        System.out.println("==============================================");

        System.out.println("==============================================");
        System.out.println("스프링 수업 중에 제목에 spring이 들어간 제목만 모아서 List로 만들기 ");
        List<String> list = springClasses.stream().filter(oc -> oc.getTitle().contains("spring"))
                        .map(OnlineClass::getTitle)
                                .collect(Collectors.toList());
        list.forEach(System.out::println);
        System.out.println("==============================================");
```

### Optional?

Optional 객체는 null이 아닌 값을 포함하거나 포함하지 않을 수 있는 컨테이너 객체이다.

Optional 객체는 자체적으로 null을 사용하면 오류가 발생할 가능성이 있는 메서드 반환 유형으로 사용하기 위한 것이다.

Optional 유형인 변수는 자체적으로 null이면 안된다! = 예측하지 못하는 null을 예방(nullsafe!)

Optional 객체 생성

* Optional.empty() : 빈 Optional 객체 생성
* Optional.of(value) : value값이 null이 아닌 경우에 사용
* Optional.ofNullable(value) : value값이 null인지 아닌지 확실하지 않은 경우에 사용

Optional이 담고 있는 객체에 접근 및 사용법

* IfPresent(함수) : Optional 객체가 non-null인 경우에 인자로 넘긴 함수를 실행 / null이면 함수 실행X
* orElse(값) : Optional 객체가 null인 경우에 인자로 넣어준 값을 리턴
* orElseGet(함수) : orElse와 같은 기능이지만 인자로 넣어준 함수가 리턴하는 것을 리턴 / null인 경우에만!!!!! 함수 실행
* orElseThrow : null인 경우에 기본 값을 리턴하지 않고 예외를 리턴할 수 있다.
