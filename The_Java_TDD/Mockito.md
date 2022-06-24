# Mockito

## Mockito란?
우선 Mock이라는 개념을 먼저 생각해보자 -> Mock이란 놀리다, 조롱하다라는 의미를 가진 영단어이다 하지만 테스트에서는 어떻게 사용하는 것일까?<br>
<br>
Mock 이란 진짜 객체처럼 동작하게 만들어서 테스트에서 사용하는 방식을 의미한다 <br>
이 객체는 이렇게 동작하게 할 것이다 이렇게 선언하는 개념이다 <br>
<br>

Mockito는 위의 Mock객체를 만들어서 제공하고 사용할 수 있게 도와주는 Mock framework이다 <br>
<br>

그래서 요놈의 용도는 어디에다가 쓰는거냐?
- DB에서 원하는 값을 넣어두고 실제 가져오면서 테스팅 가능 
- 내부의 API가 아닌 외부 API를 사용하는 경우에는 mocking을 통해서 각 값들에 대한 테스트를 진행하는 것이 가능하다
- repository도 굳이 구현하지 않고도 Mockito을 통해서 만들어두면 쉽게 사용가능

<br>

단위 테스트에 대한 개념이 조금은 애매한데 뭐 메소드를 단위라고 생각해서 해당 메소드에서 사용하는 것을 제외한 모든 것을 Mocking해야 <br>
진짜 단위 테스트라고 생각할 수 도 있고, 단순하게 어떠한 시나리오를 하나의 단위라고 생각해서 그 외의 것들 을 Mocking할 수도 있다 <br>
이러한 것은 짜는 사람 마음이라고 생각하긴 한다 <br>
<br>

원래 아는 건 @Mock을 통해서 Mocking을 진행했었다 <br>
근데 궁금한게 생겼다 -> 만약 A라는 클래스를 mock을 때려두고 그 내부의 메소드 A_method를 부르는데 A_method내부에서 B라는 클래스의 메소드 B_Method을 사용하는 것을 확인하고 싶을 때 <br>
그래서 찾게 된건 @InjectMock 이라 애노테이션이다 <br>
@Mock이 단순하게 Mock 객체를 생성하는 애노테이션이라면 <br>
@InjectMock은 미리 생성된 Mock객체를 이 애노테이션을 선언한 클래스 내부에서 미리 생성한 Mock객체를 사용하고 있는 곳에다가 넣어주겠다는 의미이다 <br>

<br><br>


## Stubbing 이란?
위에서 Mock객체를 만들고 원하는 값을 리턴 또는 가지고도록 설정해주는 것이 가능하다고 했는데 해당 하는 작업을 Stubbing이라고 해준다 <br>

기본적으로 처음 만들어지면 DEFAULT 값으로 아래와 같다
- 리턴하는 함수라면 모두 NULL을 리턴한다
- Primitive 타입은 기본 Primitive값을 리턴
- 콜렉션 타입이라면 비어있는 콜렉션을 리턴
- void 타입은 예외를 던지지 않고 아무런 일도 일어나지 않음

<br><br>

## BDD
Behaviour-Driven Development 이라는 의미이고 애플리케이션이 어떻게 행동하는지, 잘 행동하는지 테스트하는 하나의 테스트 기법이다 <br>
given - when - then <br>
given은 설정하는 단계라고 생각하면 된다 -> stub, mock, 선언등을 해주고 <br>
when은 테스트해보고 싶은 코드들을 작성하는 그러한 단계 <br>
then은 테스트한 코드들이 돌아가고 결과를 비교하고 결과값을 확인하는 단계이다 <br>
<br>