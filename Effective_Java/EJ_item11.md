# item11 - equals을 재정의하려면 hashCode도 재정의하라

\
\


equals 비교에 사용하는 정보가 변경되지 않았다면 hashcode 는 매번 같은 값을 리턴해줘야한다\
만약 두 객체에 대한 equals가 같다면 동일한 해쉬값을 가지고 있어야 한다\
물론 객체가 달라도 해쉬 값이 다를 수도 있지만 이럴때는 성능을 생각해서 다른 값을 리턴해주는 것이 좋다고 볼 수 있다\
\
\


고려해야하는 점 -> equals을 구현하는데 사용한 모든 필드들을 사용해서 hashcode을 구현해줘야 한다\
HashMap 와 같은 map 구현체를 보면, map에 값을 넣고 빼는 과정은 그냥 알아서 넣고 빼는 것이 아니라 in, out 과정에서 해쉬값을 비교한다. 그래서 hashmap 이라는 이릉이 붙어 있는 것이라고 볼 수 있다\
기본적으로 equals에서 비교가 되었다면 같은 hash값을 리턴해줘야 한다는 점을 기억하면 된다\
\


만약 다른 인스턴스인데 같은 해쉬코드가 나온다면? -> 그건 그럴 수도 있다 단지 hash collision 이 일어나서 원하지 않는 이슈가 발생할 수 있다\
위의 hashMap을 생각해보면, 같은 해쉬코드를 가진 객체를 넣어준다고 했을 때, 각각 들어가는 것이 아니라 이미 들어간 인스턴스와 함께 ArrayList 형태로 만들어져서 hashMap에 들어가게 되는\
원하지 않던 이슈를 만날 수 있다는 점이 있다\
\


그럼 어떻게 구현하냐 ->\
해쉬값을 어떻게 구하는지를 보자 그 방식은 각 필드마다의 레퍼런스타입(String, Integer, Short 등)에서 제공해주는 .haseCode() 을 통해서 값을 가져오는 것이 가능하다\
Array는 Arrays.hashCode() 등을 통해서 해쉬 값을 가져올 수 있다\
그리고 위에서 잠깐 이야기한 것 처럼 각 필드들의 해쉬값을 더함으로써 그 객체의 해쉬값을 정해주는 것이 가능하다\
아래와 같이 구현하는 것이 가능하다\
추가로, 구현되어 있는 것을 보면 눈에 띄는 것이 31인데, 31을 사용하는데에는 2가지 이유가 있다\
첫 번째는 홀수이기 때문이다 -> 짝수는 계산을 하다보면 뒤에 0이 생기는 케이스가 생기게 되고 그러면, 0이 생긴 만큼, 그 뒤의 값들이 날라가기 때문에 해쉬코드를 더 넓은 분포로 생성하는 것이 쉽지 않기 떄문이다\
두 번째는 소수이기 때문이다 -> 위대한 모 개발자 분들이 어떠한 값을 사용해서 해쉬를 작업해야 hash collision 이 적게 나는지를 확인해본 결과 그 값이 31이라는 소리가 있다\


```java
@Override public int hashCode(){
    int result = Short.hashCode(short타입의 필드);
    result = 31 * result + Integer.hashCode(int타입의 필드);
    result = 31 * result + Integer.hashCode(short타입의 필드);
    return result;
}
```

\
\


직접 해쉬코드를 구현하기 위해서는 위와 같이 구현하면 되는데, equals 와 같이 IDE의 도움을 받아서 구현하는 것도 가능하다\
아주 훌륭한 IDE의 구현방식은, Objects.hash(필드1, 필드2, 필드3) 이라는 함수를 사용해서 구현한다\
내부에 구현된 것을 보면, 똑같이 각 필드를 해싱해준다\
\
\


해쉬를 사용하는데 주의할점으로는 약간 가져다가 사용하는 개념이기 때문에\
항상 사용할 때 마다 계산을 진행하는 건 정말 효율적이지 않은 행위이다. 그래서 나오는 개념이 지연초기화와 로직적으로 내부에서 캐싱도 적용하면 좋다\
지연초기화를 사용할 때는 멀티스레드 환경을 고려해서 작성해줘야 한다 -> 멀티스레드가 되는 순간 hash collision이 날 수 있다\
해쉬의 계산로직은 api이나 문서를 통해서 밖으로 빼서 보여줄 필요는 없다\
\
\


Thread-Safety\
기본적으로 멀티 스레드에서 하나의 데이터를 바라보지 않으면 되는 것이 가장 간단한 방법이다\
\
어떻게 하면 thread-safety하게 할 수 있는가를 보면 -> 메소드의 접근지정자 뒤에 synchronized 키워드를 통해 만드는 것이다\
이렇게 synchronized 키워드를 사용하면, 이 메소드에는 한 번에 하나의 스레드만 들어오는 것이 가능하게 된다\
근데 누가봐도 문제가 많다.. 성능적으로 엄청 느려질 것이 뻔하다\
\


그래서 발전된 방식이 double checked locking 이라는게 있다고 하네\
위의 synchronized 는 메소드 자체에 lock을 걸어버리는 것인데, double checked locking 이란, 하나의 문장 단위로 락을 걸 수 있는 것이다\
사용 방법은 우리가 try-catch을 사용하는 것 처럼 synchronized 을 사용해서 내부에서 값을 락처리를 해주는 것이다\
추가로 그렇게 스레드가 고려된 변수를 사용할 때는 변수에 volatile 이라는 키워드를 사용하면 가장 최신에 업데이트된 값을 가져오게 된다\
기본적으로 캐싱을 통해서 변수의 값을 지정하는데, 그런 걸 메모리단에서 바로바로 적용되도록 도와주는 키워드이다\
\


이외에도 thread-local을 통해서도 가능하고, 불변객체(private final)을 통해서도 멀티스레드에서도 안전하게 사용하는 것이 가능하다\
아니면 생각을 바꿔서 동시에 접근하도록 허용하는 것이 concurrency 이다. 한놈이 계속해서 쓰고, 한놈이 계속해서 읽으려고 하면 불가능한 것이 기본이기 때문에 이러한 문제들을 해결하고자 이러한 것도 있다 알아두면 좋다\
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
\
\
