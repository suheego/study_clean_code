<aside>
π’ μ μ₯κ³Ό μ΄ν΄λ³Έ ν΄λ¦°μ½λ λ²μΉλ€ νμ©νμ¬ ν¨μλ€μ λΆλ¦¬νκ³  μ€λ³΅λλ λΆλΆλ€μ μ­μ νλ λ± λ¦¬ν©ν°λ§ λ²μΉμ μ μ©νλ€.
μΆκ°μ μΌλ‘ μΆμν΄λμ€, μΆμ ν©ν λ¦¬ ν¨ν΄μ μ μ©νλ€. μ΄λ² μκ°μλ **μΆμ ν΄λμ€**μ **μΆμ ν©ν λ¦¬ ν¨ν΄**μ λΉκ΅ λΆμν΄ λ³΄μ !

</aside>

### μΈν°νμ΄μ€ vs μΆμν΄λμ€

- μΈν°νμ΄μ€

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

- μΆμν΄λμ€
- **μΈν°νμ΄μ€** : κ΅¬ν μ¬ν­μ μ λ¦¬ν΄ λμ μ€κ³λλ©΄
- **μΆμν΄λμ€** : μμ§ κ΅¬νλμ§ μμ λ©μλλ€μ μ μν΄ λμ
  - μ μΈμ abstract ν€μλ μ¬μ©
  - κ΅¬ν μ¬ν­μ μ μν΄ λμ μ€κ³λλ©΄ μ¦, μΈν°νμ΄μ€μ κ°μ μ­ν 
  - μΌλ° ν΄λμ€μ²λΌ μΆκ°μ μΌλ‘ **μΌλ° λ³μ, λ©μλλ± κ΅¬νμ²΄λ₯Ό λ΄μ μ μλ€**λ νΉμ§
  - μμ λ°μ ν΄λμ€κ° μΆμ λ©μλλ₯Ό λ°λμ κ΅¬ννλλ‘ κ°μ ν  μ μλ€λ μ₯μ 

## ν©ν λ¦¬ λμμΈ ν¨ν΄

### ν©ν λ¦¬ λ©μλ ν¨ν΄

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

- μμ±ν  ν΄λμ€λ₯Ό μμ§ λͺ»ν΄λ ν΄λμ€ μμ±μ λ΄λΉνλ ν©ν λ¦¬ ν΄λμ€κ° μμ±μ λ΄λΉ
- κ°μ²΄μ μλ£νμ΄ νμν΄λμ€μμ κ²°μ  β νμ₯μ©μ΄μ±
- μλ‘μ΄ κ°μ²΄κ° μΆκ°λ  λλ§λ€ νμ ν΄λμ€λ₯Ό μΆκ°μ μΌλ‘ μ μν΄μΌ ν¨

### μΆμ ν©ν λ¦¬ ν¨ν΄

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
    super.name = "μ λ½ μ€νμΌ λν κ±΄λ¬Ό";
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

- μ°κ΄λ κ°μ²΄λ€μ μ§ν©μ νμ±
- μ¬λ¬ κ°μ²΄λ₯Ό νλμ μμ§νλ κ΅° (μ νκ΅°)μ λ§λ€ λ μ¬μ©
- μ νμ΄ μμλλ λ°©λ²μ μΆμ νμμ μλΈ ν΄λμ€μ μ μ
- νμ₯μ©μ΄μ±μ΄ λ?μ
  - μ νκ΅°μ΄ μΆκ°λ  λλ§λ€ νμ ν΄λμ€μ λͺ¨λ λ©μλ νΉμ λ³μλ€μ μΆκ°

μ°Έκ³ 

[https://niceman.tistory.com/143](https://niceman.tistory.com/143)

[https://im-yeobi.io/posts/design-pattern/factory-pattern-2/](https://im-yeobi.io/posts/design-pattern/factory-pattern-2/)
