### Effective Java 3장 객체의 생성과 삭제

#### 규칙 8. equals를 재정의할 때는 일반규약을 따르라

------

#### Equals 메서드를 재정의 하지 않아도 되는 경우

* 각각의 객체가 고유
  * 값(value) 대신 활성개체(activity entity)를 나타내는 경우 - Thread
* 클래스에 "논리적 동일성" 검사방법이 있건 없건 상관없을 때
  * java.util.Random 클래스의 경우, 굳이 이 클래스의 객체가 똑같은 난수를 만드는 객체인지 검사하는 equals 메서드를 정의할 필요성이 없음.
* 상위 클래스에서 재정의한 equals 가 하위클래스에도 적합할 때
* 클래스가 private 또는 package-private으로 선언되었고, equals 메서드를 호출할 일이 없을 때

#### Equals 메서드를 재정의 해야할 때

* 객체동일성(같은 객체인지)이 아닌 논리적 동일성(가지고 있는 값 등이 같은지)의 개념을 지원하는 클래스일 경우
  * Date, Integer 등의 값(value) 클래스
  * 단, enum과 같은 클래스나, 최대 하나의 객체만 존재하게 만드는 객체의 경우, 객체동일성이 곧 논리적 동일성임.
* 상위 클래스의 equals 가 하위클래스의 필요를 충족하지 못할 경우

#### Equals 메서드를 정의할 때의 일반규약

##### 반사성

* null 이 아닌 참조 x가 있을 때, x.equals(x) 는 true

##### 대칭성

* null 이 아닌 참조 x,y 가 있을 때, x.equals(y) 는 y.equals(x) 가 true 일 때만 true를 반환

```java
public final class CaseInsensitiveString {
	private final String s;
	
	public CaseInsensitiveString(String s){
		...
	}
	
	//대칭성 위반
	@Override public boolean equals(Object o){
		if(o instance of CaseInsensitiveString)
			return s.equalsIgnoreCase((CaseInsensitiveString)o).s);
		if(o instance of String) 
			return s.equalsIgnoreCase((String) o);
	}
}
```

* 위 코드는 한 방향으로만 동작하여 아래와 같은 결과를 나타냄(대칭성 깨짐)

```java
CaseInsensitiveString cis = new CaseInsensitiveString("Polish");
String s = "polish";

cis.equals(s) // true
s.equals(cis) // false
```

* 개선

```java
@Override public Boolean equals(Object o){
	return o instance of CaseInsensitiveString &&
	        ((CaseInsensitiveString) o).s.equalsIgnoreCase(s);
}
```

##### 추이성

* null 이 아닌 참조 x,y,z 가 있을 때, x.equals(y) 가 true 이고, y.equals(z) 가 true 이면, x.equals(z) 도 true이다.

##### 일관성

* null 이 아닌 참조 x, y가 있을 때, equals 를 통해 비교되는 정보에 아무 변화가 없다면, x.equals(y) 호출 결과는 호출 횟수에 상관없이 항상 같아야 함.

##### null 이 아닌 참조 x 에 대해, x.equals(null) 은 항상 false