# 클래스, 객체, 인터페이스
### 클래스 계층 정의
코틀린에서의 인터페이스 안에는 추상 메소드 뿐만 아니라 구현이 있는 메소드도 구현할 수 있다. 하지만 인터페이스에는 필드가 들어갈 수는 없다 <br>
자바에서 인터페이스, 추상메소드를 설정해주는 과정이 각각 implements, extends 키워드를 통해서 진행했었는데, 코틀린에서는 단순하게 :(콜론)만 붙여서 클래스의 확장과 인터페이스의 구현을 넣을 수 있다 <br>
여기서 추상메소드를 받아서 override 을 하는 과정에서 사실 자바에서는 무조건 사용할필요는 없었지만 코틀린에서는 무조건 @Override 요 애노테이션을 붙혀야만 한다. 상위 클래스에 있는 메소드와 시그니처가 같은 메소드를 우연히 하위 클래스에서 선언하는 경우에는 컴파일이 안되기 때문에 override나 메소드 이름으로 잘 구분해줘야 한다 <br>
인터페이스도 default 구현을 하는 것이 가능은 하다 변수에서 간단하게 디폴트를 넣어주는 것처럼 = 을 통해서 기능을 작성해주면 된다 <br>
<br>
상속한 인터페이스를 호출할때도 기존에 자바에서는 리턴타입.super.사용할함수() 이렇게 사용했었는데 코틀린에서는 super<리턴타입>.사용함수() 이렇게 사용한다 <br>
코틀린은 기본적으로 자바 6와 호환되도록 설계되었다 그래서 인터페이스에 디폴트 메소드를 설정하는 자바 7인가 8에 들어간 기능이라서 따로 지원하지 않는다 따라서 코틀린은 디폴트 메소드가 인터페이스를 일반 인터페이스와 디폴트 메소드 구현이 정적 메소드로 들어있는 클래스를 조합해 구현한다 <br>
<br><br>

자바에서 final로 상속을 금지하지 않는 모든 클래스를 다른 클래스가 상속할 수 있다 <br>
취약한 기반 클래스라는 문제는 하위 클래스가 기반 클래스에 대해 종속적으로 가진 기능에 대해서 만약 기반 클래스가 변경됨으로써 깨지는 문제를 의미한다 그래서 어떤 클래스가 자신을 상속하는 방법에 대해 제한적인 규칙을 주지 않는 한, 
그 클래스의 클라이언트는 생성자의 의도에 맞게 override 해서 사용하지 않을 가능성이 있다 <br>
그래서 effective java 에서는 특별하게 하위 클래스에서 재정의를 통해서 기능을 활용할 가능성이 있는 클래스가 아니라면 final 을 통해서 제한하라 라고 하고 있다 <br>
이러한 철학을 코틀린에도 적용되어서 코틀린에서의 클래스와 메소드는 기본적으로 final 이다 <br>
어떠한 클래스의 상속을 허용하기 위해서는 open 이라는 키워드를 통해서 재정의를 가능하게 해줘야한다 그리고 추가로 재정의를 허용하고 싶은 메소드나 필드에도 open을 붙혀줘야 한다 <br>
참고로 클래스에 open 을 붙히고 내부의 함수에서 재정의를 제한하고 싶다면 final 키워드를 붙혀줘야 하는 점을 기억하자 <br>
```kotlin
open class OVRIDE : myInterface{
    final override fun test()
    override fun test2()
}
```
이렇게 사용한다 <br><br>

코틀린에서 제공해주는 상속 제어 변경자들은 아래와 같다
- final : 요 키워드가 있으면 재정의할 수 없으며, final 키워드는 클래스 멤버의 기본 변경자이다
- open : 요 키워드가 있으면 재정의할 수 있으며, 이게 있어야 재정의가 가능하다
- abstract : 이게 있으면 무조건 재정의해줘야 한다 
- override : 상위 클래스나 상위 인스턴스의 멤버를 재정의한다는 의미

<br><br>

다음은 접근 제어자이다 <br>
일단 자바와 비슷하게 public, protected, private 이렇게 2가지가 있다 <br> 
자바에서는 기본적으로 private이지만 코틀린에서는 기본적으로 public 이다 추가로 코틀린에서는 그 3개가 아니라 internal 이라는 접근 제어자도 존재한다 <br>
internal 이라는 접근 제어자는 모듈 내부에서만 볼 수 있다는 의미이다 이 제어자가 있다는 의미는 모듈의 구현에 대해서 진정한 캡슐화를 제공하겠다는 의미이다 <br>
리스트로 해서 보자 <br>
- public : 적지 않는 디폴트로는 public 이고 클래스 멤버에서 작성하면 모든 곳에서 사용할 수 있고, 최상위에 선언하면 모든 곳에서 사용할 수 있다
- internal : 클래스 멤버로는 같은 모듈 안에서만 사용힐 수 있다. 그리고 최상위에 선언하면 같은 모듈 안에서만 사용할 수 있다
- protected : 클래스 멤버로는 하위 클래스 안에서만 사용할 수 있다. 그리고 최상위에서는 선언할 수 없다
- private : 같은 클래스 안에서만 사용할 수 있다. 그리고 최상위에 선언하면 같은 파일 안에서만 사용할 수 있다.

<br>

참고로 protected 멤버는 오직 어떤 클래스나 그 클래스를 상속한 클래스 안에서만 보인다 
클래스를 확장한 함수는 그 클래스의 private이나 protected 멤버에 접근할 수 없다 <br>
또한 코틀린에서는 외부 클래스가 내부 클래스나 중첩된 클래스의 private 멤버에 접근할 수 없다는 점이다 <br>
<br>

코틀린에서 클래스 내부에서 다른 클래스를 선언하는 중첩 클래스가 존재한다. 근데 특징이 있다면 
중첩 클래스는 명시적으로 요청하지 않는 한 바깥쪽 클래스 인스턴스에 대한 접근할 수 없다는 것이 특징이다 <br>
코틀린 중첩 클래스는 아무런 변경자가 안붙으면 자바의 static 중첩 클래스와 같다 -> 그래서 만약에 내부 클래스로 변경해서 바깥쪽 클래스에 대한 참조를 포함하고 싶다면 inner 키워드를 통해서 명시해줘야 한다 <br>
예시로 클래스 B가 있고 그 안에 A가 있을 때 A에 대해서 <br>
- 중첩 클래스(바깥쪽 클래스에 대한 참조를 저장하지 않음) : 자바에서는 static class A 이렇게 적고 코틀린에서는 class A
- 내부 클래스(바깥쪽 클래스에 대한 참조를 저장함) : 자바에서는 class A 이렇게 적고 코틀린에서는 inner class A 
<br>

참고로 inner 안에서 상위 클래스의 참조에 접근하려면 this@Outer 이러한 키워드를 사용해서 접근해야 한다 
<br><br>

클래스 계층을 정의 시 계층 확장을 제한할 수 있다 <br>
코틀린 컴파일러에서는 when에서 항상 default 으로 else 분기를 넣어줘야만 한다 <br>
그래서 대부분 default 으로는 예외를 던져서 처리하곤한다 <br>
근데 사실 좀 귀찮기도하고 default 분기가 있으면 클래스 계층에 새로운 하위 클래스를 추가하더라도 컴파일러가 모든 경우를 처리하는지 판단하기 쉽지 않다 <br>
그래서 이 방법을 해결하기 위해서는 sealed 이라는 클래스를 사용할 수 있다 <br>
sealed 이라는 키워드를 붙히면, 그 상위 클래를 상속한 하위 클래스 정의를 제한하는 것이 가능하다 <br>
즉, sealed 클래스의 하위 클래스를 정의할 때는 반드시 상위 클래스 안에 중첩을 시켜주어야 한다 <br>
when 에서 sealed 클래스의 모든 하위 클래스를 처리한다면, default 분기가 필요없다 
