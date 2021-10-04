# 💡 제 3장 함수

## 1. 작게 만들어라

### 1-1. 한 가지 기능만 해라

- 함수는 되도록 적은 양의 코드로 한 가지 기능만을 가진 함수가 좋다. 그렇다면 얼마나 짧아야 할까? 2-5줄 정도가 사실 매우 좋다. 함수가 길면 길수록 이는 한 가지 기능 이상의 것을 하고 있다는 반증이기도 한 셈이니까.

### 1-1. 부수 효과를 일으키지 마라

- 아래 checkPassword 함수는 겉보기에 패스워드가 맞는 지 확인하는 함수인 것처럼 보이지만 사실 세션 정보를 초기화 하는 부수적인 효과를 일으킨다. 이름만 봐서는 세션을 초기화 한다는 사실이 들어나지 않는다. 또한 그 사실을 숨기고 있는 것이 더 큰 위험을 초래한다.

```java
public class UserValidator {
	private Cryptograher cryptographer;

	public boolean checkPassword (String userName, String password) {
		User user = UserGateway.findByName(userName);
		if (user != User.NULL) {
			String codePhrase = user.getPhraseEncodedByPassword();
			String phrase = cryptographer.decrypt(codePhrase, password);
			if ("Valid Password".equals(phrase)) {
				Session.initialize(); /// 세션 정보 초기화 !?
				return true;
			}
		}
		return false;
	}

}
```

## 2. 함수 당 추상화 수준은 하나로

- 함수가 확실히 한 가지 기능을 하기 위해서는 해당 함수 모든 문장의 추상화 수준은 하나로 동일해야한다. 한 함수 내에 추상화 수준을 섞으면 코드를 읽는 사람이 헷갈린다. 특정 표현이 근본 개념인지 아니면 세부사항인지 뒤섞기 시작하면, 깨어진 창문처럼 사람들이 함수에 세부사항을 점점 더 추가한다.

### 2-1. 위에서 아래로 코드 읽기: 내려가기 규칙

코드는 위에서 아래로 이야기처럼 읽혀야 좋다. 한 함수 다음에는 추상화 수준이 한 단계 낮은 함수가 온다. 즉, 위에서 아래로 프로그램을 읽으면 함수 추상화 수준이 한 번에 한 단계씩 낮아진다. 코드를 읽는 것이 하나의 문단을 읽고 다음 문단을 자연스레 내려가 이해되듯 자연스럽게 읽혀야 한다. 즉, 함수의 모든 문장이 추상화 수준이 복잡하게 얽히지 않아 있어야 한다.

## 3. 함수 인수는 적을 수록 좋다

- X, Y 좌표와 같은 두 개의 인수가 하나의 개념을 표현하는 경우가 아니라면 최대한 함수 인수는 3개보다 2개가 2개보다 1개, 1개보다 0개가 좋다. 인수가 많을 수록 함수 로직의 경우의 수가 많아져 테스트 코드를 짤 때에도 모든 경우를 고려하여 짜야 하기 때문에 전체적으로 복잡성과 함수에 대한 볼륨이 커져 side effect 가 커진다.

## 4. 서술적인 이름을 사용하라

- 함수가 작고 단순할수록 서술적인 이름을 짓기도 쉬워진다. 짧고 이해하기 어려운 이름일수록 실제 함수 코드를 읽기 전부터 답답해지거나 매우 기나긴 함수를 읽고도 현재 상황을 해결할 수 없는 상황을 인지하게 되는 순간 숨이 턱 막혔던 그런 경험 한 번쯤 개발자라면 없을까. 협업에 있어서도 함수의 이해도를 높일 수 있는 서술적인 함수명이 훨씬 효율적이다.

## 5. 명령과 조회를 분리하라

- 함수는 뭔가를 수행하거나 뭔가에 답하거나 둘 중 하나만 해야한다. 둘 다 하는 경우 혼란을 초래한다.

```java
public boolean set (String attribute, String value);
if (set("username", "unclebob")) ...
if (setAndCheckIfExits("username", "unclebob"))..
```

- 위의 set 함수는 attribute 속성을 찾아 값을 value로 설정한 후, 성공하면 true 혹은 실패하면 false 를 반환한다. 실제 함수를 호출하는 코드를 읽어보자면 username 을 unclebob 으로 설정한다는 것인지 혹은 설정 되어있는지 확인하는 것인지 알기 어렵다. 그렇기엔 setAndCheckIfExists 라고 함수명을 변경하여 함수의 기능을 명확히 하는 것도 좋지만 if 문에 넣어 봐도 그리 자연스럽지 못하다.

```java
if (attributExists("username")) {
	setAttribute("username", "unclebob");
}
```

- 위의 함수는 username이 있는지 확인하는 조회 기능을 attributExists 에 부여해 확인한 뒤, setAttribute 함수를 통해 해당 unclebob 이라는 값을 username에 설정할 수 있도록 한다. 이렇게 조회와 명령을 분리하는 것이 함수를 읽어 내려가며 이해하기에 혼란을 야기하지 않는다.

## 6. 오류 코드보다 예외를 사용하라 ( try...catch )

- 함수에서 오류 코드를 반환하는 방식은 명령/조회 분리 규칙을 위반한다.

```java
if (deletePage(page) == E_OK) {
	if (registry.deleteReference(page.name) == E_OK) {
		if (configKeys.deleteKey(page.name.makeKey())) == E_OK) {
...
```

- 위 코드는 동사 / 형용사 혼란을 일으키지 않는 대신 여러 단계로 중첩되는 코드를 야기한다. 오류 코드를 반환하면 호출자는 오류 코드를 곧 바돌 처리해야 한다는 문제에 부딪힌다.

```java
try {
	deletePage(page);
	registry.deleteReference(page.name);
	configKeys.deleteKery(page.name.makeKey());
} catch (Exception e) {
	logger.log(e.getMessage());
}
```

- try/catch 함수로 오류 처리 코드가 원래 코드에서 분리되어 코드가 깔끔해진다. 그러나 코드 구조에 혼란을 일으키면 정상 동작과 오류 처리 동작을 뒤섞는다.

### try/ catch 블록을 별도 함수로 분리하라

```java
public void delete (Page page) {
	try {
		deletePageAndAllReferences(page);
	} catch (Exception e) {
		logError(e);
	}
}

private void deletePageAndAllReferences (Page page) throws Exception {
	deletePage(page);
	registry.deletePageAndAllReferences(page.name);
	configKeys.deleteKey(page.name.makeKey());
}

private void logError(Exception e) {
	logger.log(e.getMessage());
}
```

- try/catch 로 분리 가능한 삭제 기능을 가진 delete 함수를 새로 만들고 deletePageAndAllReferences 라는 함수를 통해 실제 페이지를 삭제하는 기능을 수행하고, 에러를 반환할 수 있는 구조로 분리해 내는 것이 훨씬 깔끔하고 이해하기 수월하다.
