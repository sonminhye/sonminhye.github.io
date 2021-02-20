### Effective Java 2장 객체의 생성과 삭제

#### **규칙 2. 생성자 인자가 많을 경우, Builder 패턴 적용하라**

------

##### 생성자와 정적팩터리의 한계

* **선택적 인자**가 많은 상황에 놓였을 때, 잘 적응하지 못함. (꼭 초기화 되지 않아도 되는 인자)

##### 대안 1. 점층적 생성자 패턴

* 필수인자만 받는 생성자 정의, 선택적 인자를 추가
* (예시) 영양분석표를 클래스로 나타낸 상황이며, servingSize, servings 만 필수인자

NutritionFacts.java

```java
public class NutritionFacts{
  private final int servingSize;
  private final int servings;
  private final int calories;
  private final int fat;
  private final int sodium;
  
  public NutritionFacts(int servingSize, int servings){
    this(servingSize, servings);
  }
  
  public NutritionFacts(int servingSize, int servings, int calories){
    this(servingSize, servings, calories);
  }
  ...
}
```

Client

```java
NutritionFacts nf = new NutritionFacts(240, 8, 100);
```

- 인자수가 늘어나면 클라이언트 코드를 작성하기 힘들며, 읽기 어려운 코드가 됌.

##### 대안 2. 자바빈 패턴

* 인자없는 생성자로 객체를 생성한 후, Set 메소드로 인자 초기화
* 1회의 함수 호출로 객체를 끝낼 수 없어 객체일관성 깨짐
* 변경 불가능한 클래스를 만들 수 없음(규칙15)

#### 빌더 패턴

* 점층적 생성자 패턴의 안정성 + 자바빈 패턴의 가독성 결합
* 빌더 클래스는 만들고자 하는 클래스의 정적 멤버클래스로 정의

NutritionFacts.java

```java
public class NutritionFacts{
	private final int servingSize;
	private final int servings; // 필수인자
	private final int calories = 0; // 선택적 인자는 기본값 초기화
	....
	
	public static class Builder{
		private final int servingSize; // 필수인자
		private final int servings; // 필수인자
		private final int calories = 0; // 선택적 인자는 기본값 초기화
		...
		
		public Builder(int servingSize, int servings){
			this.servingSize = servingSize;
			this.servings = servings;
		}
		
		public Builder calories(int val){
			calories = val; return this;
		}
		....
      
    public NutritionFacts build(){
      return new NutritionFacts(this);
    }
    
	}
	
	private NutritionFacts(Builder builder){
		servingSize = builder.servingSize;
		servings = builder.servings;
		calories = builder.calories;
		...
	}
}
```

Client

```java
NutritionFacts nf = new NutritionFacts.Builder(240,8).calories(20).build();
```

- 매개변수의 유효성 검사도 build 함수안에 추가 가능.

##### 단점

* 객체 생성하려면 빌더 객체를 우선 생성해야 함. (오버헤드 가능성)
* 점층적 생성자 패턴보다 코드가 길어 인자가 많은 상황에서 사용하여야 함.

https://johngrib.github.io/wiki/builder-pattern/