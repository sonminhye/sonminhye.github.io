### Effective Java 2장 객체의 생성과 삭제

#### **규칙 3. private 생성자나 enum 자료형은 싱글턴 패턴 따르도록 설계하라**

------

#### 싱글턴

* 객체를 하나만 만들 수 있는 클래스

**방법 1. static 멤버변수로 선언(생성자를 private으로)**

```java
public class Elvis{
  	public static final Elvis INSTANCE = new Elvis();
  	private Elvis(){...}
}
```

* 클래스가 싱글턴인지 선언만 보면 금방 알 수 있어 좋음.
* public static final 선언하였으므로 같은 객체 참조.
* JVM의 경우, 정적팩토리 메서드 호출을 inline으로 처리하므로 정적 팩터리 메서드 방법을 사용하는게 좋음.

**방법 2. 정적 팩토리 메서드 이용(변수 선언은 private으로)**

```java
public class Elvis{
  	private static final Elvis INSTANCE = new ELVIS();
  	private Elvis(){...}
  	public static Elvis getInstance() {return INSTANCE;}
}
```

* API 를 변경하지 않고도 싱글턴 적용을 선택할 수 있음. ex) getInstance 메소드만 수정하면 스레드마다 별도의 객체를 반환하도록 할 수도 있음.
* 제네릭 타입 수용이 쉬움(규칙27)

**방법 3. 직렬화 가능한 클래스로 싱글턴 구현(11장)**

* implements Serializable 추가만 한다면, 역직렬화 시 새로운 객체가 생성됌.
* 모든 필드 transient 로 선언, readResolve 메소드 추가필요

```java
private Object readResolve(){
	// 동일한 객체가 반환되도록 하는 동시에, 가짜 Elvis객체는 GC 가 처리하도록 만듬.
	return INSTANCE;
}
```

**방법 4. (JDK 1.5부터) Enum 자료형 정의**

```java
public enum Elvis{
		INSTANCE;
		...
    public String getName(){
      	return "minemi";
    }
}
```

```java
String name = Elvis.INSTANCE.getName();
```

https://github.com/jojoldu/blog-code/tree/master/enum-settler