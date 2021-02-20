### Effective Java 3장 객체의 생성과 삭제

#### 규칙 11. clone을 재정의 할 때는 신중하라

------

#### Cloneable 인터페이스

* clone 메서드가 구현되어 있지 않으나, Object의 메소드가 어떻게 동작할지에 대해 결정하는 역할
* Cloneable 인터페이스를 구현한 객체만이 clone 메소드를 override 했을 때 복제가능하며, 구현되어있지 않으면 CloneNotSupportedException을 던짐.

***Object의 clone 메소드의 경우, 호출한 객체와 동일한 객체를 새로 만들어 반환한다.***



#### clone 메서드의 일반규약

1. x.clone() != x
2. x.clone().getClass() == x.getClass()
3. x.clone().equals(x)



#### clone 메서드 재정의 시 고려할 점

##### 비-final 클래스에 clone을 재정의할 때는 반드시 super.clone()을 호출하여 얻은 객체를 반환해야함

- 만약, new 연산자를 통해 새로운 객체를 만든다면, 그 클래스를 상속받은 하위클래스에서 clone() 을 구현해 super.clone()을 호출했으면, 상위클래스 타입이 반환되게 된다.
- 따라서, 상위클래스가 이 규칙을 따르면, 해당 상위클래스 역시 super.clone() 을 호출해 하위클래스와 동일한 객체를 새로 만들어 반환할 것이다.



##### Cloneable 인터페이스를 구현하는 클래스는 제대로 동작하는 public clone 메서드를 제공해야 함.

* 보통 모든 필드가 기본 자료형(int, double 등)이거나, 변경 불가능한 객체(immutable) 참조라면 문제없이 복사
* 복사할 객체가 변경 가능한 객체에 대해 참조하는 필드를 가지고 있다면, clone 메소드가 제대로 동작하지 않을 수도 있음.

```java
public class Stack{
	private Object[] elements;
	private int size;
	
	.....
	
	@Override public Stack clone(){
		try{
			Stack result = (Stack) super.clone();
			result.elements = elements.clone(); // 다음처럼, elements 배열또한 clone() 해줘야함.
		}
	}
}
```

* 단, clone() 메소드는 final 로 선언된 필드의 일반적인 용법과 호환되지 않는데, 이를 복제가능하게 만들기 위해 final 선언을 제거해야 할 수도 있다.
* 직접 구현한 자료구조의 경우, 참조의 참조값을 갖는 경우도 있는데, 이럴경우엔 deep copy를 지원하는 기능을 구현해야 한다.
* 다중스레드에 안전해야 하는 클래스를 Cloneable 하게 만들기 위해, clone메서드에도 동기화 매커니즘을 적용하여야 함.



#### 복사생성자와 복사팩토리

```java
public Yum(Yum yum); // 복사 생성자

public static Yum newInstance(Yum yum); // 복사 팩토리
```

* 언어외적인 객체 생성수단에 의존하지 않아도 됌
* 제대로 문서화 되어있지 않고, 어려운 규약에 맞추지 않아도 됌.
* 해당메서드가 정의된 클래스가 구현하는 인터페이스를 인자로 받을 수 있음.
  * 모든 일반용도의 Collection 구현체는, Map이나 Collection을 인자로 받는 생성자를 제공한다.
* 배열을 복사할 때는 clone() 이 적합하나, 그 외에는 사용하지 않는 것이 좋다.

https://lannstark.tistory.com/100