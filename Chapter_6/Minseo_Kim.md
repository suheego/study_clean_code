## 자료 추상화

### 구체적인 클래스 1

```java
public class Point {
	public double x;
	public double y;
}
```

### 구체적인 클래스 2

```java
public interface Vehicle {
	double getFuelTankCapacityInGallons();
	double getGallonsOfGasoline();
}
```

### 추상적인 클래스 1

```java
public interface Point {
	double getX();
	double getY();
	void setCartesian(double x, double y);
	double getR();
	double getTheta();
	void setPolar(double r, dluble theta);
}
```

### 추상적인 클래스 2

```java
public interface Vehicle {
	double getPercentFuelRemaing();
}
```

> 자료를 세세하게 공개하기 보다는 추상적인 개념으로 표현하는 편이 좋다.

- 변수를 private로 선언해 감추는 것이 전부는 아니다.
- 인터페이스나 조회/설정 함수만으로 추상화가 이뤄지지 않는다.
- 각 값마다 조회/설정 함수를 제공한다면 구현을 외부로 노출하는 셈이다.
- 두 Vehicle 클래스를 비교하면 구체적인 클래스는 해당 클래스의 기능을 모두 드러내며기능을 한정짓는다.

## 자료/객체 비대칭

- 객체는추상화를 통해 직접적인 자료를 숨긴 채 인터페이스를 통해 자료를 다루는 함수만 공개한다.
- 자료구조는 자료를 그대로 공개하며 별다른 함수를 제공하지 않는다.
  - 때로 개발자들은 자료와 객체의 비대칭을 이해하지 못한 채 자료구조의 형태를 그대로 갖춘 추상화 없는 객체 즉, 껍데기 뿐인 객체를 무의미하게 양산해 낸다.

## 디미터 법칙

- 모듈은 자신이 조작하는 객체의 속사정을 몰라야 한다는 법칙
- 객체에서 허용된 메서드가 반환하는 새로운 객체의 메서드 호출해서는 안된다.
  - 기차 법칙, 조잡하다. ⇒ 분리해라
  - 디미터 법칙을 어기는지 확인하기 위해서는 반환하는 객체 혹은 어떠한 값이 객체인지 자료구조인지 확인하면 알 수 있다.
  - 객체는 때로 잡종 구조가 되기 쉽다. ⇒ 이러한 양상으로 새로운 자료 구조를 추가하거나 변경하기 쉽지 않게 된다.

## 자료 전달 객체 (DTO)

- 자료 구조체의 전형적인 형태로 공개 변수만 있고 함수가 없는 클래스
- 데이터베스트에 저장된 자료와 같은 가장 가공되지 않은 객체 1차원적인 형태 별다른 이익 X

```java
public class User {
	private String firstName
	private String lastName
	private Number age

	public getFullName() {
		return this.firstName + this.lastName
	}
}
```

### 활성 레코드

- DTO의 특수한 형태
- 탐색 함수 ( select 후 자료 변환 ) 존재
- 비즈니스 규칙 메서드를 추가해 이런 객체를 객체로 취급하지만 이는 잡종 구조에 불과

## 결론

- 객체
  - 동작을 공개 자료를 숨김
  - 기존 동작을 변경하지 않으면서 새 객체 타입을 추가하기는 쉬운 반면, 기존 객체에 새 동작을 추가하기는 어렵다.
- 자료구조
  - 별다른 동작 없이 자료를 노출
  - 반대로 기존 자료 구조에 새 동작을 추가하기는 쉬우나, 기존 함수에 새 자료 구조를 추가하기는 어렵다.
- 시스템 구현에 있어, 새로운 자료 타입을 추가하는 유연성이 필요하다면 객체가 적합하고, 새로운 동작을 추가하는 유연성이 필요하다면 자료 구조와 절차적 코드가 더 적합하다. 더 적합한 구조를 유연하게 사용하자
