# 주석

주석은 필요악이다.

프로그래밍 언어를 치밀하게 사용해 의도를 표현할 능력이 있다면, 주석은 거의 필요하지 않다.



## 주석을 추가하는 이유

### 나쁜 코드 품질

- 모듈 구성이 좋지 않기 때문에 주석을 남길 수 밖에 없다.
- 주석은 **나쁜 코드를 보완하지 못한다.**

### 명확하지 않은 의도 표현

- 코드만으로 의도를 표현하기 힘든 경우 때문에 주석을 단다.
- 하지만 조금만 더 고민해보면, 코드로 의도를 표현할 수 있다.

```java
// 직원에게 복지 혜택을 받을 자격이 있는지 검사한다.
if ((employee.flags & HOURLY_FLAG)) && (employee.age > 65))


if (employee.isEligibleForFullBenefits())
```



**코드를 깔끔하게 정리하고 표현력을 강화해서 애초에 주석이 필요하지 않도록 코드를 짜자!**



## 좋은 주석

정말 좋은 주석은 주석을 달지 않는 방법을 찾아낸 주석이다!



### 법적인 의미

회사가 정립한 구현 표준에 맞춰 법적인 이유로 주석을 넣으라고 할 때

- 소스 파일 첫머리에 표준 주석 헤더를 넣는 경우가 있다.
- 다만, 주석으로 표현하는 것보다 가급적이면 라이선스 문서나 외부 문서로 참조하는 방향이 더 깔끔하다.

```java
// Copyright (c) 2003,2004,2005 by Object Mentor, INC. All right reserved.
// GNU general Public License 버전 2 이상을 따르는 조건으로 배포한다.
```



### 정보 제공

기본적인 정보를 주석으로 제공할 때

```java
// 테스트 중인 Reponder 인스턴스를 반환한다.
	protected abstract Responder responderInstance();
```

하지만 주석으로 기본 정보를 반환할 수 있어도 함수 이름에 정보를 담는게 더 좋다.

```java
public void responderBeingTested() {
	protected abstract Responder responderInstance()
}
```

정규 표현식의 예제

```java
// kk:mm:ss EEE, MMM dd, yyyy 형식이다.
Pattern timeMatcher = Pattern.compile(
	"\\\\d*:\\\\d*:\\\\d* \\\\w*, \\\\w* \\\\d*, \\\\d*");
```



### 의도를 설명할 때

구현을 이해하게 도와주는 선을 넘어서 결정에 깔린 의도까지 설명할 때

```java
// 스레드를 대량 생성하는 방법으로 어떻게든 경쟁 조건을 만들려 시도한다.

for (int i =0; i <25000; i++) {
	WidgetBuilderThread widgetBuilderThread = 
		new WidgetBuilderThread(widgetBuilder, text, parent, failFlag);
	Thread thread = new Thread(widgetBuilderThread);
	thread.start();
}
assertEquals(false, failFlag.get());
}
```



### 의미를 명료하게 밝힐 때

인수 반환값이 표준 라이브러리나 변경하지 못하는 코드에 속해서 의미를 명확하게 밝혀야할 때

- 주석이 올바른지 검증하기 쉽지 않기 때문에, 주석을 달 때 고민해야할 케이스이다.

```java
public void testCompareTo() throws Exception {
	wikiPagePath a = PathParser.parse("PageA");
	wikiPagePath ab = PathParser.parse("PageA.PageB");
	wikiPagePath b = PathParser.parse("PageB");
	wikiPagePath aa = PathParser.parse("PageA.PageA");
	wikiPagePath bb = PathParser.parse("PageB.PageB");
	wikiPagePath ba = PathParser.parse("PageB.PageA");
	
	assertTrue(a.compareTo(a) == 0); // a == a
	assertTrue(a.compareTo(b) != 0); // a != b
	assertTrue(ab.compareTo(ab) == 0); // ab == ab
	assertTrue(a.compareTo(b) == -1); // a < a
	assertTrue(aa.compareTo(ab) == -1); // aa < ab
	assertTrue(ba.compareTo(bb) == -1); // ba < bb
	assertTrue(b.compareTo(a) == 0); // b > a
	assertTrue(ab.compareTo(aa) == 1); // ab > aa
	assertTrue(bb.compareTo(ba) == 1); // ba > ba

}
```



### 결과를 경고할 때

다른 프로그래머에게 결과를 경고할 목적

```java
public static SimpleDataFormat makeStandartHttpDateFormat {
	// SimpleDateFormat은 스레드에 안전하지 못하다.
	// 따라서 각 인스턴스를 독립적으로 생성해야 한다.
	SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");
	df.setTimeZone(TimeZone.getTimeZone("GMT"));
	return df;
}
```



### TODO 

- 앞으로 할 일을 TODO 주석으로 남길 때 → 당장은 구현하기 어려운 업무를 기술

#### TODO 주석을 달만한 예시

- 더 이상 필요없는 기능을 삭제하라는 알림
- 누군가에게 문제를 봐달라는 요청
- 더 좋은 이름을 떠올려달라는 부탁
- 앞으로 발생할 이벤트에 맞춰 코드를 고치라는 유의

→ 하지만 이것이 나쁜 코드를 만들 핑계로 두면 안된다.

IDE에서 TODO 주석을 보여주는 기능이 있기 때문에 주기적으로 점검해서 작업하거나 삭제하는 것이 좋다.



### 중요성을 강조

중요성이 적어보이는 것을 강조하기 위해서 주석을 사용

```java
String listItemContent = match.group(3).trim();
// 여기서 trim은 정말 중요하다. trim 함수는 문자열에서 시작 공백을 제거한다.
// 문자열에 시작 공백이 있으면 다른 문자열로 인식되기 때문이다.

new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```



### 공개 API의 docs

#### Java Docs?

- Java 소스에 문서화를 하는 방법
- 클래스나 메소드에 주석으로 기술한 내용을 **javadoc** 명령어나 또는 이를 이용한 빌드 툴(maven의 pharse 등)을 사용하여 문서화
- /** 두 개로 시작하고 */ 로 끝나야 한다.

#### 다른 언어는?

- [JavaScript Docs](https://jsdoc.app/)
- [Python Docstring](https://www.python.org/dev/peps/pep-0257/)



## 나쁜 주석

### 특별한 의미가 없는 주석

이해가 안되어 다른 모듈까지 뒤져야하는 주석은 읽는 사람을 배려하지 못한다.

```java
public void loadProperties()
{
	try
	{
		String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
		FileInputStream propertiesStream = nwe FileInputStream(propertiesPath);
		loadedProperties.load(propertiesStream);
	}
catch(IOException e)
	{
		// 속성 파일이 없다면 기본 값을 모두 메모리로 읽어 들였다는 의미다.
	}
}
```



### 같은 이야기를 반복하는 주석

같은 코드 내용을 중복하는 주석

```java
// this.closed가 true일 때 반환되는 유틸리티 메서드다.
// 타임아웃에 도달하면 예외를 던진다.

public synchonized void waitForClose(final long timeoutMillis)
throws Exception
{
	if(!closed)
	{
	wait(timeoutMillis);
	if(!closed)
		throw now Exception("MockReponseSender could not be closed");
	}
}
```



### 오해할 여지가 있는 정보를 가진 주석

잘못된 정보 전달을 하는 주석



### 의무적으로 단 주석

아무런 가치 없이 달린 주석

```java
/**
*
* @param title CD 제목
* @param author CD 저자
* @param tracks CD 트랙 숫자 
* @param durationInMinutes CD 길이(단위: 분)
*/
public void addCD(String title, String author, int tracks, int durationInMinutes){

    Cd cd = new CD(); 
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.durationInMinutes = durationInMinutes; 
    cdList.add(cd); 
}
```



### 이력을 기록하는 주석

작업 히스토리를 관리하는 로그성 주석

- 이전에는 모듈 헤더에 로그성으로 주석을 남겼지만, 이제는 git으로 관리하자



### 함수나 모듈로 표현된 것을 별도로 생성한 주석

동일한 의미이기 때문에 필요 없는 주석이다.

```java
// 전역목록 <smodule>에 속하는 모듈이 우리가 속한 하위 시스템에 의존하는가?
if(smodule.getDependSubsystems().contains(subSysMod.getSubSystem())
```

```java
ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))`
```



### 위치를 표시한 주석

소스 파일에서 특정 파일의 위치를 표시하는 주석 → 가독성을 낮추는 원인

```java
// Actions ////////////////////////////////
```



### 닫는 괄호에 다는 주석

캡슐화된 함수에는 주석이 잡음 → 차라리 함수를 줄이자.

```java
try{
  while((line = in.readLine) != null){
    lineCount++;
    charCount += line.length();
    String words[] = line.split("a");
    wordCount += words.length;
  } //while
  System.out.println("wow");
  System.out.println("wow");
  System.out.println("wow");
} //try
```



### 공로를 돌리거나 저자를 표시하는 주석

git으로 누가 무엇을 추가했는지 이력이 관리되기 때문에, 별도로 작성할 필요가 없다.

```java
/* 고수희가 추가함 */
```



### 주석으로 처리한 코드

주석으로 처리한 코드는 다른 사람에게 혼선을 준다. 바로 삭제하자

```java
//    public void run(ApplicationArguments args) throws Exception {
//        StopWatch stopWatch = new StopWatch();
//        stopWatch.start();
//
//        stopWatch.stop();
//    }
```



### HTML 주석

HTML 주석은 IDE에서 읽기 어렵다.



### 전역 정보

시스템의 전반적인 내용을 담은 것



### 너무 많은 정보

모듈과 관련없는 정보일 때



### 모호한 관계

주석과 주석을 표현하는 코드의 관계가 모호할 때

```java
/*
 * 모든 픽셀을 담을 만큼 충분한 배열로 시작 (여기에 필터 바이트를 더한다.)
 * 그리고 헤더 정보를 위해 200바이트를 더한다.
 */
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200]
```



### 함수 헤더

짧고 한 가지만 수행하여 이름을 잘 붙인다면 헤더에 표현할 필요가 없다.



## 부정확한 주석

주석을 달 것이면, 명확한 주석을 달자.

- 부정확한 주석은 아예 없는 주석보다 훨씬 더 나쁘다.
- 부정확한 주석은 독자를 현혹하고 오도한다.
- 부정확한 주석은 결코 이뤄지지 않을 기대를 심어준다.