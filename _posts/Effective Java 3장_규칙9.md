### Effective Java 3장 객체의 생성과 삭제

#### 규칙 9. equals 를 재정의할 때는 반드시 hashCode도 재정의하라

------

#### HashCode의 일반규약

* 응용 프로그램 실행 중 같은 객체의 hashCode 는 언제나 동일한 정수가 반환되어야 함
* equals(Object) 가 같다고 판정한 두 객체의 hashCode 객체의 값은 같아야 함
* equals(Object) 가 다르다고 판정한 두 객체의 hashCode 값은 무조건 다를 필요는 없음. 단, 서로 다른 hashCode 값이 나오면 해시테이블의 성능이 향상될 수 있다.

#### hashCode 재정의 시 주의점

성능개선을 위해 객체 중요부분을 해시코드 계산 과정에서 생략하면 안됌.