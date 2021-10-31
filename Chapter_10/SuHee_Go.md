# 클래스

- 특정 객체를 생성하기 위해 변수와 메소드를 정의하는 일종의 틀
- 객체를 정의 하기 위한 상태(멤버 변수)와 메서드(함수)로 구성

## 클래스 체계

### private, protect, public

- 캡슐화의 장점인 정보 은닉(Information Hiding) 방식
- 클래스의 변수, 함수에 대해 접근을 제어할 수 있음

### 접근 순서

Python과 Javascript는 모든 클래스가 public

private → protected → public

- 변수 목록
  - 정적 공개 상수 (public static)
  - 정적 비공개 변수 (private static)
  - 비공개 인스턴스 변수 (private)
- 공개 함수 (public method)
- 비공개 함수 (private method)
  - 자신을 호출하는 공개 함수 직후에 위치

### 캡슐화(Encapsulation)

- 객체의 실제 구현을 외부로부터 감추는 방식
- 클래스를 개발할 때 구현을 감추고, 외부 객체와 상호작용하는 부분만 노출
  - 외부에서의 잘못된 사용 방지

## 작은 클래스

클래스를 만들 때 첫 번째 규칙도, 두 번째 규칙도 클래스 크기가 작아야한다.

- 클래스 이름은 해당 클래스의 책임을 기술해야한다.
  - 간결한 이름이 떠오르지 않으면 클래스 크기가 큰 것이다.
- 클래스 설명은 만일(if), 그리고(and), 하며(or),하지만(but)을 사용하지 않고서 25단어 내외로 가능해야한다.

**AS-IS**

```java
public class SuperDashboard extends JFrame implements MetaDataUser {
    public String getCustomizerLanguagePath()
    public void setSystemConfigPath(String systemConfigPath) 
    public String getSystemConfigDocument()
    public void setSystemConfigDocument(String systemConfigDocument) 
    public boolean getGuruState()
    public boolean getNoviceState()
    public boolean getOpenSourceState()
    public void showObject(MetaObject object) 
    public void showProgress(String s)
    public boolean isMetadataDirty()
}
```

**TO-BE**

```java
public class SuperDashboard extends JFrame implements MetaDataUser {
    public Component getLastFocusedComponent()
    public void setLastFocused(Component lastFocused)
    public int getMajorVersionNumber()
    public int getMinorVersionNumber()
    public int getBuildNumber() 
}
```

### 단일 책임의 원칙 (Single Responsibility Principle)

- 클래스나 모듈을 변경할 이유가 하나, 단 하나뿐이어야한다.
- 변경할 이유를 파악하려고 애쓰다보면 코드를 추상화하기 쉬워진다.
- 작은 클래스는 각자 맡은 책임이 하나이고, 변경할 이유가 하나이고, 다른 작은 클래스와 협력해 시스템에 필요한 동작을 수행한다.

```java
public class Version {
	public int getMajorVersionNumber()
	public int getMinorVersionNumber()
	public int getBuildNumber()
}
```

### 응집도 (Cohesion)

- 한 모듈 내에 존재하는 함수, 데이터 등의 구성 요소들 사이의 밀접한 정도
- 클래스는 인스턴스 변수 수가 작아야한다.
- 각 클래스 매서드는 클래스 인스턴스 수를 하나 이상 사용해야한다.
- 메서드가 변수를 더 많이 사용할수록 메서드와 클래스는 응집도가 높다.
- 응집도가 높다 → 클래스에 속한 메서드와 변수가 서로 의존하며 논리적 단위로 묶인다는 의미
- 응집도가 낮다 → 클래스에 속한 메서드나 변수들에 불필요한 것이 존재하거나, 관련성이 적다.

### 응집도를 유지하면 작은 클래스 여럿이 나온다.

- 큰 함수를 작은 함수 여럿으로 나누기만 해도 클래스 수가 많아진다.

### 변경하기 쉬운 클래스

- 대다수의 시스템은 지속적인 변경이 가해진다. → 뭔가 변경할 때마다 프로그램이 의도대로 동작하지 않을 위험이 따른다.
- 깨끗한 시스템은 클래스를 체계적으로 변경해 수반하는 위험을 낮춘다.

### 변경으로부터 격리

- 요구사항은 변하고 코드도 변한다.
- 구체적 클래스(concrete)
  - 상세한 구현(코드)을 포함
- 추상 클래스(abstract)
  - 개념만 포함
- 추상클래스를 이용하여 구현이 미치는 영향을 최소화하자