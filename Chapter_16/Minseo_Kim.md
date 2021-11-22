<aside>
📢 앞 장과 살펴본 클린코드 법칙들 활용하여 함수들을 분리하고 중복되는 부분들을 삭제하는 등 리팩터링 법칙을 적용한다.
추가적으로 추상클래스, 추상 팩토리 패턴을 적용한다. 이번 시간에는 **추상 클래스**와 **추상 팩토리 패턴**을 비교 분석해 보자 !

</aside>

### 인터페이스 vs 추상클래스

- 인터페이스

```java
interface Creature {
    abstract void run ();
    abstract void move ();
}

public class Person implements Creature {
	abstract void run { ... }
	abstract void move { ... }
}
```

```java
abstract class Animal {
	abstract void makeSound();
}

public class Lion extends Animal {
	void makeSound() {
		System.out.print("Miau");
	}
}
```

- 추상클래스
- **인터페이스** : 구현 사항을 정리해 놓은 설계도면
- **추상클래스** : 아직 구현되지 않은 메서드들을 정의해 놓음
  - 선언시 abstract 키워드 사용
  - 구현 사항을 정의해 놓은 설계도면 즉, 인터페이스와 같은 역할
  - 일반 클래스처럼 추가적으로 **일반 변수, 메서드등 구현체를 담을 수 있다**는 특징
  - 상속 받은 클래스가 추상 메서드를 반드시 구현하도록 강제할 수 있다는 장점

## 팩토리 디자인 패턴

### 팩토리 메소드 패턴

```java
public interface Shape {
	void draw()
}

public Circle implements Shape {

	@Override()
	public void draw() {
		System.out.print("Circle - Shape draw");
	}
}

public Rectangle implements Shape {

	@Override()
	public void draw() {
		System.out.print("Rectangle - Shape draw");
	}
}

public Square implements Shape {

	@Override()
	public void draw() {
		System.out.print("Square - Shape draw");
	}
}

public class ShapeFactory {

	public Shape getShape (String shapeType) {

		switch (shapeType) {
			case 'circle':
				return new Circle();
			case 'rectangle':
				return new Rectangle();
			case 'square':
				return new Square();
			default:
				return null
		}
	}
}
```

- 생성할 클래스를 알지 못해도 클래스 생성을 담당하는 팩토리 클래스가 생성을 담당
- 객체의 자료형이 하위클래스에서 결정 ⇒ 확장용이성
- 새로운 객체가 추가될 때마다 하위 클래스를 추가적으로 정의해야 함

### 추상 팩토리 패턴

```java
public interface BuildingMaterialsFactory {
  public Cement createCement();
  public Wood createWood();
}

public class EuropeMaterialsFactory implements BuildingMaterialsFactory {
  public Cement createCement() {
    return new MixtureCement();
  }

  public Wood createWood() {
    return new WalnutWood();
  }
}

public class AsiaMaterialsFactory implements BuildingMaterialsFactory {
  public Cement createCement() {
    return new PortlandCement();
  }

  public Wood createWood() {
    return new OakWood();
  }
}

public class EuropeStyleUniversity extends Building {
  private BuildingMaterialsFactory materialsFactory;

  public EuropeStyleUniversity(BuildingMaterialsFactory materialsFactory) {
    this.materialsFactory = materialsFactory;
    super.name = "유럽 스타일 대학 건물";
  }

  @Override
  public void buildFoundation() {
    System.out.println("Build a foundation");
    super.cement = materialsFactory.createCement();
    super.wood = materialsFactory.createWood();
  }
}

public class EuropeConstructionFirm extends ConstructionFirm {
  @Override
  public Building getBuildingInstance(BuildingType type) {
    Building building = null;

    BuildingMaterialsFactory materialsFactory = new EuropeMaterialsFactory();

    switch (type) {
      case HOSPITAL:
        building = new EuropeStyleHospital(materialsFactory);
        break;
      case UNIVERSITY:
        building = new EuropeStyleUniversity(materialsFactory);
        break;
    }

    return building;
  }
}
```

- 연관된 객체들의 집합을 형성
- 여러 객체를 하나의 응집화된 군 (제품군)을 만들 때 사용
- 제품이 생상되는 방법은 추상 형식의 서브 클래스에 정의
- 확장용이성이 낮음
  - 제품군이 추가될 때마다 하위 클래스에 모두 메서드 혹은 변수들을 추가

참고

[https://niceman.tistory.com/143](https://niceman.tistory.com/143)

[https://im-yeobi.io/posts/design-pattern/factory-pattern-2/](https://im-yeobi.io/posts/design-pattern/factory-pattern-2/)
