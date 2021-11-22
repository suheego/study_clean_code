## JUnit 을 통한 리팩터링 과정 살펴보기

## 조건문으로 캡슐화 하라

- 조건문을 캡슐화하고 알기 쉬운 함수명으로 캡슐화
  - 이해하기가 훨씬 수월해진다.
- 부정문을 긍정문으로 변경
  - 부정문은 이해하기가 긍정문보다 어렵다.

```java
if (expected == null || actual == null || areStringsEqual())
```

```java
public String compact (String message) {
	if (shoudNotcompact()) {
		...
	}
}

private boolean shoulNotCompact() {
	return expected == null || actual == null || areStringEqual();
}
```

```java
public String compact (String message) {
	if (canBeCompacted()) {
		...
	}
}

private boolean canBeCompacted() {
	return expected != null || actual != null || !areStringEqual();
}
```

## 명확한 변수명

- class 멤버 변수명에 범위를 나타낼 필요가 없다.
  - 명확한 변수명

```java
private int fContextLength;
private String fExpected;
private String fActual;
private int fPrefix;
private int fSuffix;
```

```java
private int contextLength;
private String expected;
private String actual;
private int prefix;
private int suffix;
```

- class 내 지역 변수와 별개로 compact 함수의 지역 변수임을 명확히

```java
public String compact(String message) {
    if (canBeCompacted()) {
      return Assert.format(message, fExpected, fActual);
    }
    findCommonPrefix();
    findCommonSuffix();
    String expected = compactString(fExpected);
    String actual = compactString(fActual);
    return Assert.format(message, expected, actual);
 }
```

```java
public String compact(String message) {
    if (canBeCompacted()) {
      return Assert.format(message, expected, actual);
    }
    findCommonPrefix();
    findCommonSuffix();
    String compactExpected = compactString(expected);
    String compactActual = compactString(actual);
    return Assert.format(message, compactExpected, compactActual);
 }
```

## 함수는 한 가지 기능만 ! 함수명으로 기능을 !

### 이전 코드

- compact 함수는 함수명으로만 따졌을 때 조건 없이 문자열을 압축하는 것처럼 보인다.
- 그러나, 실제 Compact 함수
  - 압축이 가능한 지 확인하는 오류 점검
  - 단순 압축 문자열 x ⇒ 형식이 갖추어진 문자열 반환

```java
public String compact(String message) {
    if (canBeCompacted()) {
      return Assert.format(message, expected, actual);
    }
    findCommonPrefix();
    findCommonSuffix();
    String compactExpected = compactString(expected);
    String compactActual = compactString(actual);
    return Assert.format(message, compactExpected, compactActual);
 }

private boolean canBeCompacted() {
	return expected != null || actual != null || !areStringEqual();
}
```

### 수정 이후 코드

- formatCompactedComparision
  - 해당 문자열을 압축하여 형식에 맞춰 값을 반환
- compatExpectedAndActual
  - 실제 문자열을 비교해 가며 압축하는 함수
- canBeCompacted
  - 압축 가능한 지 확인
- 완벽하게 함수는 한 가지 기능만 하도록 분리되고 함수명도 기능에 맞춰 재명명 되었다.

```java
public String formatCompactedComparision(String message) {
    if (canBeCompacted()) {
			compatExpectedAndActual();
      return Assert.format(message, expected, actual);
    } else {
			return Asssert.format(message, expected, actual);
		}
 }

private boolean compatExpectedAndActual() {
		findCommonPrefix();
    findCommonSuffix();
    String compactExpected = compactString(expected);
    String compactActual = compactString(actual);
}

private boolean canBeCompacted() {
	return expected != null || actual != null || !areStringEqual();
}
```

## 일관적인 함수 호출

- compactString 는 문자열을 반환 하지만 findCommonPrefix, findCommonSuffix 는 x

```java
private boolean compatExpectedAndActual() {
		prefixIndex = findCommonPrefix();
    suffixIndex = findCommonSuffix();
    String compactExpected = compactString(expected);
    String compactActual = compactString(actual);
}

private void findCommonPrefix() {
    int prefixIndex = 0;
    int end = Math.min(expected.length(), fActual.length());
    for (; prefixIndex < end; prefixIndex++) {
        if (expected.charAt(prefixIndex) != actual.charAt(prefixIndex)) {
            break;
        }
    }
		return prefixIndex

 }

private void findCommonSuffix() {
	int expectedSuffix = expected.length() - 1;
	int actualSuffix = actual.length() - 1;
	for (; actualSuffix >= prefixIndex && expectedSuffix >= prefixIndex; actualSuffix--, expectedSuffix--) {
		if (expected.charAt(expectedSuffix) != actual.charAt(actualSuffix)) {
			break;
		}
	}
	return expected.length() - expectedSuffix;
}
```

## 숨겨진 시간적 결함을 고려하라

- findCommonSuffix 는 findCommonPrefix 를 먼저 계산한다는 사실에 의존

### 1차 수정

- findCommonSuffix 함수를 prefixIndex 를 인수로 사용하도록 변경
  - 호출 순서는 정해지더라도 prefixIndex 인수가 그닥 필요한 이유 x
  - 다른 개발자가 고칠 가능성도 있음

```java
private boolean compatExpectedAndActual() {
		prefixIndex = findCommonPrefix();
    suffixIndex = findCommonSuffix(prefixIndex);
    String compactExpected = compactString(expected);
    String compactActual = compactString(actual);
}

private void findCommonSuffix(int prefixIndex) {
	int expectedSuffix = expected.length() - 1;
	int actualSuffix = actual.length() - 1;
	for (; actualSuffix >= prefixIndex && expectedSuffix >= prefixIndex; actualSuffix--, expectedSuffix--) {
		if (expected.charAt(expectedSuffix) != actual.charAt(actualSuffix)) {
			break;
		}
	}
	return expected.length() - expectedSuffix;
}
```

### 2차 수정

- 호출 순서를 확실히 하기 위해서 findCommonPrefixAndSuffix 함수 선언
- 접미어의 길이를 뜻하는 것이므로 index ⇒ length

```java
private void findCommonPrefixAndSuffix() {
    findCommonPrefix();
    int suffixLength = 1;
    for (; suffixOverlapsPrefix(suffixLength); suffixLength++) {

				if (
						charFromEnd(expected, suffixLength) !=
						charFromEnd(actual, suffixLength)
				) {
            break;
        }
    }
    suffixIndex = suffixLength;
}

private char charFromEnd(String s, int i) {
	return s.charAt(s.length() - i);
}

private boolean suffixOverlapsPrefix(int suffixLength) {
	return actual.length() - suffixLength < prefixLength ||
		expected.length() - suffixLength < prefixLength;
}
```
