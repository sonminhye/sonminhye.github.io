### Effective Java 2장 객체의 생성과 삭제

#### **규칙 1. 생성자 대신 정적 팩터리 메서드를 사용하라**

------

##### 장점

-  **생성자와 달리 정적팩터리 메서드의 경우, 이름을 정할 수 있음. (이해가 쉬움)**

  - 인자의 순서만 다른 생성자 두 개 존재 시, 이름이 동일하므로 API설명서를 읽지 않고는 이 두개를 구분하기 어려움. 정적 팩터리 메서드의 경우, 이름으로 의미를 표현 가능하므로 이런 문제가 발생하지 않음.
  - (예시) 소수일 가능성이 높은 BigInteger객체를 생성하는 생성자인 BigInteger(int, int, Random) -> BigInteger.probablePrime 과 같은 이름의 정적 팩터리 메서드로 표현했다면 더 이해하기 쉬웠을 것.

- **생성자와 달리, 호출될 때 마다 새로운 객체를 생성하지 않아도 된다.**

  - 이미 만들어둔 객체 사용 / 객체 캐시(Cache) 해두고 재사용
  - Boolean.valueOf(Boolean) 메서드 - flyWeight패턴과 유사

  ```java
  public static Boolean valueOf(Boolean b){
  	return b ? Boolean.TRUE : Boolean.FALSE;
  }
  ```

- **생성자와 달리 반환값 자료형의 하위자료형 객체를 반환 가능**

  - public 으로 선언되지 않은 클래스의 객체도 반환가능(유연성)
  - java.util.EnumSet 의 경우, 모두 정적 팩터리 메소드만 존재. (enum 상수 개수에 따라 두 개 구현체 가운데 하나를 선택하여 반환) 
    - enum 상수 < 64개 : RegularEnumSet 반환
    - enum 상수 > 64개 : JumboEnumSet 반환

##### 단점

* **public이나 protected로 선언된 생성자가 없으므로, 하위클래스를 만들 수 없음.**
* **정적 팩터리 메서드가 다른 정적 메서드와 확연히 구분되지 않음.**
  * 주석을 통해 해당 메서드가 정적 팩터리 메서드임을 알리거나
  * 이름을 지을 때 주의하는 방법 뿐
    * 보통 정적 팩토리 메서드 이름은 valueOf, of, getInstance, newInstance, getType, newType 의 단어로 표현함.