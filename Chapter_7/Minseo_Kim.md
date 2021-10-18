깨끗한 코드와 깨끗한 오류 처리는 매우 밀접하게 관련이 있다 !

## 오류 코드보다 예외를 사용하라

### 오류 코드를 사용한 예시

```java
public class DeviceController {
	...
	public void sendShutDown() {
		DeviceHandle handle = getHandle(DEV1);
		if (handle != DeviceHandle.INVALID) {
		...
		if (record.getStatus() != DEVICE_SUSPENDED) {
		...
```

### 예외를 사용한 예시

```java
public class DeviceController {
	...
	public void sendShutDown() {
		try {
			tryToShutDown();
		} catch (DeviceShutDownError e) {
			logger.log(e);
		}
	}

	public tryToShutDown() throws DeviceShutDownError {
		DeviceHandle handle = getHandle(DEV1);
		DeviceRecord record = retrieveDeviceRecord(handle);

		pauseDevice(handle);
		clearDeviceWorkQueue(handle);
		closeDevice(handle);
	}

	...
```

- 개발자 정의의 오류 코드는 는 코드의 복잡도를 상승시켜 가독성이 떨어진다.
- 오류 코드 대신 예외를 정의하고 발생 시키는 코드로 복잡도를 낮추자.

## Try-Catch-Finally 문부터 작성하라

- try-catch 문은 트랜잭션과 비슷한 구조로 이해할 수 있다. 프로그램 상태를 일관적으로 유지 시킬 수 있게 하기 때문이다.
- 예외가 발생할 코드를 짤 때는 try-catch-finally 문을 사용하여 코드의 큰 그림을 그리는 용도로 사용하라

## 미확인(unchecked) 예외를 사용하라

> **_미확인 에러_** : 런타임 에러
> **_확인된 예외_** : 컴파일 단계에서 확인되는 에러. 메서드가 반환할 예외를 모두 열거해놓아야 함

- 확인된 예외는 확장에는 열려있되, 수정에는 닫혀 있어야 한다는 OCP(Open Closed Principle, 개방 폐쇄의 원칙)를 위반한다.
- 확인된 예외는 대규모 시스템에서 메서드 선언된 부분에 모두 추가해야 해서 모듈을 다시 빌드해 재배포 해야하는 비용이 발생한다.

## 예외에 의미를 제공하라

- 오류 메시지에 실패한 연산 이름과 실패 유형도 등의 정보를 담아 예외와 함께 던진다.
- 로깅 시스템을 사용해 배포 이후에도 반복되는 에러 처리를 확인해 볼 수 있다.

## 호출자를 고려해 예외 클래스를 정의하라

- 우리가 오류를 처리하는 방식
  - 오류를 기록 ⇒ 시스템을 계속 수행해도 좋은지 확인
  - 상당히 일정
- 외부 라이브러리가 반환할 예외를 잡는 클래스를 정의해 외부 라이브러리를 감싼 뒤, try-catch-finally 문으로 중복을 피하라

```java
public class LocalPort {
	private ACMEPort innerPort;

	public LocalPort (int portNumber) {
		innerPort = new ACMEPort(portNumber);
	}

	public void open() {
		try {
			innerPort.open();
		} catch (DeviceResponseException e) {
			throw new PortDeviceFailure(e);
		} catch (ATM1212UnlockedException e) {
			throw new PortDeviceFailure(e);
		} catch (GMXError e) {
			throw new PortDeviceFailure(e);
		}
	}
	...
}

```

## 정상 흐름을 정의하라

```java
try {
	MealExpencses expenses = expenseReportDAO.getMeals(employee.getID());
	m_total += expeneses.getTotal();
} catch (MealExpensesNotFound e) {
	m_total += getMealPerDiem();
}
```

```java
public class PerDiemMealExpenses implements MealExpenses {
	public int getTotal() {
	// 기본값으로 일일 기본 식비를 반환한다.
	}
}

MealExpencses expenses = expenseReportDAO.getMeals(employee.getID());
m_total += expeneses.getTotal();
```

- 외부 API 를 감싸 독자적인 예외를 던지고, 코드 위에 처리기를 정의해 중단된 계산 처리한다. 때로는 중단이 적합하지 않을 때도 있다.
- 클래스를 만들거나 객체를 조작해 특수 사례를 오류로 잡지 않아도 정상 흐름을 따라가도록 하면 된다.

## null을 반환하거나 전달하지 마라

- NullPointerException이 발생하는 경우를 최대한 피하는 코드로 작성해 모든 변수마다 null 인지 아닌지 확인하는 실수를 유발하는 구성으로 코드를 짜지 마라
