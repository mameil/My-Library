# 값 타입과 불변 객체

_값 타입이라는 것은 단순하게 사용하려고 만든건데 만약 여러 객체에서 공유되면?_





## 값 타입 공유 참조

가정을해보자

만약 2개의 객체에서 하나의 값 타입을 공유하고 있는 상황에서

객체 1이 값 타입을 받아서 사용하고 있는 상황에서

객체 2가 값 타입을 수정하고 그 수정한 값을 사용하면

```java
member1.setHomeAddress(new Address("123", "456"));
Address address = member1.getHomeAddress();

address.setLatitude("9999");
member2.setHomeAddress(address);
```

객체 1은 계속해서 과거의 값을 사용하고 있을까? → 정답은 **아니요**



두 객체가 하나의 인스턴스를 참조하고 있기 때문에 영속성 컨텍스트에서 객체 1, 2 모두의 값 타입에 대한 속성이 사라진 것으로 판단해서 두 객체 모두에게 update를 쳐준다.

그래서 member2의 latitude만 9999로 변환된 것을 기대했지만 실제로는 member1의 latitude도 9999로 변환되어있다.

어떻게 보면 와 이걸 찾아서해주네? 이지만 어떻게 보면 내가 시키지도 않은 일을 해버렸네?이다.

그래서 이러한 것을 부작용이라고도 부른다.

그러면 객체를 직접 참조(공유)하지말고 복사해서 사용하자!





## 값 타입 복사

그러면 공유하지말고 인스턴스를 복사해서 사용하자!

```java
member1.setHomeAddress(new Address("1234", "5678"));
Address address = member1.getHomeAddress();

Address newAddress = address.clone();

newAddress.setLatitude("9999");
member2.setHomeAddress(newAddress);
```

member2에게 새로운 주소를 할당해주기 위해서 clone을 사용했고

이렇게 복사를 사용하면 위처럼 알아서 되는 것이 아니라 의도대로 member2의 latitude만 정상적으로 변경되어있다.

이렇게 항상 값을 복사해서 사용하면 공유 참조와는 다르게 의도대로 값을 수정할 수 있다. 하지만 문제는 **임베디드 타입은** 기본 타입과 같은 int, string등이 아니라 **객체라는 점이다**

**그래서 정말 값을 복사해서 사용하는 것이 아닌 이상 객체의 공유 참조를 피할 수 없는 노릇이다**

**아니면 공유 참조된 객체에는 setter를 제거해서 수정할 수 없도록 하던가**





## 불변 객체

위에 구구절절은 결국

**값 타입은 될 수 있으면 불변 객체로 설계해야하는 이유에 대해서 이야기 한 것이다.**

필요성은 위에서 이야기를 했고,

불변 객체 : 한 번 만들면 절대로 변경할 수 없는 객체를 의미한다.

그래서 조회는 계속해서 할 수 있지만 수정은 불가능하다는 점

**그리고 참조 값을 공유해도 인스턴스의 값이 수정될 일이 없으니까 부작용이 발생하지 않는다는 점**

말이 거창한 불변 객체이지만 어렵게 생각할 게 아닌게 단순하게 Getter는 만들어두고, Setter를 만들지 않는 것이 뽀인트이다.













