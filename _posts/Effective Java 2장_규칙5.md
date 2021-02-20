### Effective Java 2장 객체의 생성과 삭제

#### 규칙 5. 불필요한 객체는 만들지 말라

------

**기능적으로 동일한 객체는 필요 시마다 만드는 것보다는 재사용하는 것이 좋음.**

``````java
// 안좋은 예
String s = new String("stringette"); // 실행 시마다 String 객체를 만듬.
// 개선
String s = "stringette"; 
``````

* 생성자나 정적팩터리 메서드(규칙1)를 함께 제공하는 클래스의 경우, 정적팩터리 메서드를 사용하면 불필요한 객체 생성을 피할 수 있다. (ex) 규칙1 의 Boolean.valueOf 의 예



**변경가능한 객체도 재사용 가능. 단, 최초 한 번 초기화 되면 변경 불가능해야 함.**

* final static 멤버변수 선언, 정적 초기화 블록 사용.

```java
public class Person{
		private final Data birthDate;
  
		private static final Date BOOM_START;
		private static final Date BOOM_END;
		
		static {
			Calendar gmtCal = Calendar.getInstance(TimeZone.getTimeZone("GMT"));
			gmtCal.set(1946, Calendar.JANUARY, 1,0,0,0);
			BOOM_START = gmtCal.getTime();
			...
			BOOM_END = gmtCal.getTime();
		}
		
		public boolean isBabyBoomer(){
			return birthDate.compareTo(BOOM_START) >= 0 &&
			       birthDate.compareTo(BOOM_END) < 0;
		}
}
```



**(JDK 1.5) 자동객체화(AutoBoxing)**

* primitive type을 해당하는 객체로 자동 변환해주는 방식 ex) long -> Long객체

```java
Long num = 0L;
for (long i = 0; i < Integer.MAX_VALUE; ++i) {
    sum += i;
}
```

* 객체 표현형 대신 기본 자료형을 사용하여, 생각지 못한 AutoBoxing이 일어나지 않도록 유의해야 함.



**객체풀(object pool) 생성**

* 극단적으로 객체생성 비용이 높지 않으면 사용하지 않는 것이 좋음. ex) DB Connection
* 독자적인 객체 풀을 만들면 코드가 복잡해지고, 메모리 요구량이 증가하며 성능 떨어짐.