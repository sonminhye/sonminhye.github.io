### Effective Java 3장 객체의 생성과 삭제

#### 규칙 12. Comparable 구현을 고려하라

------

#### CompareTo

* compareTo 메서드는, Object 에 구현되지않았으며, Comparable 인터페이스에 있는 유일한 메소드

* 단순 동치검사 외 순서비교가 가능한 메서드임. 거의 모든 자바 플랫폼 라이브러리에 포함된 값 클래스는 해당 인터페이스를 구현한다고 보면 됌.



#### compareTo 메서드 일반규약

일반적으로 해당 규약을 가지는데, 이는 규칙8의 equals 와 매우 유사함.

1. **sgn(x.compareTo(y) == -sgn(y.compareTo(x)))**

2. **x.compareTo(y) > 0 && y.compareTo(z) > 0 이면, x.compareTo(z) > 0**

3. **x.compareTo(y) == 0 이면, sgn(x.compareTo(z) == sgn(y.compareTo(z)))**

4. **x.compareTo(y) == 0  == x.equals(y) 를 만족하기를 권장**

일반적으로 한 클래스 내 자연적인 순서가 존재하기만 하면, 규약들을 거의 만족한다고 보면 된다. 

이 compareTo 클래스의 규약을 어기는 클래스의 경우, 클래스의 오동작을 야기할 수 있으니 주의해야 한다.



compareTo() 규약을 만족하며 값 변수(?)를 추가하기 위해 해당 클래스를 상속하는 방법은 없음. 

대신, 새로운 클래스를 만들고 원래의 클래스를 멤버변수로 추가하여 해당 클래스를 볼 수 있는 view 메소드를 만들어주면 클라이언트는 원할 때 원래 클래스 형태로도 이용가능하다(?)



#### compareTo 메서드와 equals 의 차이

* Comparable 인터페이스는 자료형을 인자로 받는 **제네릭 인터페이스**. 
  * 따라서, 메서드의 인자 자료형은 컴파일 시간에 정적으로 결정되어 서로 다른 클래스 객체에 대해 비교를 고려할 필요가 없으며, 서로 비교 불가한 클래스가 인자로 넘어왔을 때 ClassCastException 예외가 던져진다.

* null이 인자로 넘겨질 경우는 NullPointerException 이 던져진다.



#### compareTo 메서드의 구현

* 보통, 객체 참조 필드는 compareTo 메서드를 재귀적으로 호출해 비교.

* 비교할 필드가 Comparable을 구현하지 않고 있거나, 특이한 순서를 이용해야 하는 경우에는 Comparator 를 명시적으로 사용가능하다.

  * 직접 구현
  * 이미 있는 Comparator 사용가능

  ```java
  public final class CaseInsensitiveString implements Comparable<CaseInsensitiveString>{
  	public int compareTo(CaseInsensitiveString cis){
  		return String.CASE_INSENSITIVE_ORDER.compare(s, cis.s);
  	}
  	//..
  }
  ```

* 비교필드 값의 차이를 반환하는 경우, 비교할 필드가 음수가 아닐 때만 사용가능하며, 필드값 최소치와 최대치의 차가 Integer.MAX_VALUE(2^31-1) 이하인 경우에만 사용가능함.

  * 해당 부호비트가 있는 32비트 정수 안에 부호비트가 있는 임의의 32비트 정수 두 개의 차를 온전히 담을 수 없어 일반규약 1, 2 를 위반하게 됌.

