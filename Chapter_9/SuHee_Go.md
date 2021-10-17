# TDD

Test Driven Development (테스트 주도 개발)

- 반복 테스트를 이용한 소프트웨어 방법론
- 작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현하는 것

## TDD의 이점

- 설계를 개선하기 때문에 개발 속도를 높여준다 → 실수가 줄고 결함을 더 빨리 찾을 수 있기 때문
- 코드 유연성, 유지보수성, 재사용성을 제공한다. → 단위테스트
  - 변경이 쉽다.

## TDD 법칙 3가지

1. 실패하는 단위 테스트를 작성할 때 까지 실제 코드를 작성하지 않는다.
2. 컴파일은 실패하지 않으면서, 실행이 실패하는 정도만 단위 테스트를 작성한다.
3. 현재 실패하는 테스트를 통과할 정도만 실제 코드 작성

- 개발과 테스트가 30초 주기로 묶임
- 테스트 케이스가 늘어날 수록 관리 이슈가 생기게 됨

# 깨끗한 테스트 코드

- 테스트 코드는 실제 코드 못지않게 중요하다 → 실제 코드와 마찬가지로 깨끗하게 짜야한다.
- 단위 테스트 슈트는 설계와 아키텍처를 최대한 깨끗하게 보존한다.
- 깨끗하지 않은 테스트 코드는 코드 구조를 개선하는 능력이 떨어진다. → 수정한 코드가 제대로 도는지 확인하기도 힘들다.

## 깨끗한 테스트 코드 짜기

- 가독성이 중요하다 → 실제 코드보다 테스트 코드에 더더욱 중요
  - 명료성, 단순성, 풍부한 표현력이 필요하다.
- 테스트 코드는 정말 필요한 자료 유형만 함수만 사용한다.

### 도메인에 특화된 언어로 테스트 코드를 구현

- 특정 분야에 최적화된 프로그래밍 언어 (Domain Specific Language)

- 도메인 특화 언어는 우리가 알고 있는 범용 프로그래밍 언어 (C, Java, Python)과 반대되는 개념

- 도메인 수준에서 검증과 확인이 가능하다는 이점이있다.

- **Build-Operate-Check Pattern**

  - **Build**: 테스트 데이터 를 빌드
  - **Operate :** 데이터에 대해 **연산** 을 수행
  - **Check :** 작업 결과를 확인

- AS-IS

  ```java
  public void testGetPageHieratchyAsXml() throws Exception {
    crawler.addPage(root, PathParser.parse("PageOne"));
    crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
    crawler.addPage(root, PathParser.parse("PageTwo"));
  
    request.setResource("root");
    request.addInput("type", "pages");
    Responder responder = new SerializedPageResponder();
    SimpleResponse response =
      (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
    String xml = response.getContent();
  
    assertEquals("text/xml", response.getContentType());
    assertSubString("<name>PageOne</name>", xml);
    assertSubString("<name>PageTwo</name>", xml);
    assertSubString("<name>ChildOne</name>", xml);
  }
  
  public void testGetPageHieratchyAsXmlDoesntContainSymbolicLinks() throws Exception {
    WikiPage pageOne = crawler.addPage(root, PathParser.parse("PageOne"));
    crawler.addPage(root, PathParser.parse("PageOne.ChildOne"));
    crawler.addPage(root, PathParser.parse("PageTwo"));
  
    PageData data = pageOne.getData();
    WikiPageProperties properties = data.getProperties();
    WikiPageProperty symLinks = properties.set(SymbolicPage.PROPERTY_NAME);
    symLinks.set("SymPage", "PageTwo");
    pageOne.commit(data);
  
    request.setResource("root");
    request.addInput("type", "pages");
    Responder responder = new SerializedPageResponder();
    SimpleResponse response =
      (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
    String xml = response.getContent();
  
    assertEquals("text/xml", response.getContentType());
    assertSubString("<name>PageOne</name>", xml);
    assertSubString("<name>PageTwo</name>", xml);
    assertSubString("<name>ChildOne</name>", xml);
    assertNotSubString("SymPage", xml);
  }
  
  public void testGetDataAsHtml() throws Exception {
    crawler.addPage(root, PathParser.parse("TestPageOne"), "test page");
  
    request.setResource("TestPageOne"); request.addInput("type", "data");
    Responder responder = new SerializedPageResponder();
    SimpleResponse response =
      (SimpleResponse) responder.makeResponse(new FitNesseContext(root), request);
    String xml = response.getContent();
  
    assertEquals("text/xml", response.getContentType());
    assertSubString("test page", xml);
    assertSubString("<Test", xml);
  }
  ```

- TO-BE

  ```java
  public void testGetPageHierarchyAsXml() throws Exception {
    makePages("PageOne", "PageOne.ChildOne", "PageTwo");
  
    submitRequest("root", "type:pages");
  
    assertResponseIsXML();
    assertResponseContains(
      "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
  }
  
  public void testSymbolicLinksAreNotInXmlPageHierarchy() throws Exception {
    WikiPage page = makePage("PageOne");
    makePages("PageOne.ChildOne", "PageTwo");
  
    addLinkTo(page, "PageTwo", "SymPage");
  
    submitRequest("root", "type:pages");
  
    assertResponseIsXML();
    assertResponseContains(
      "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>");
    assertResponseDoesNotContain("SymPage");
  }
  
  public void testGetDataAsXml() throws Exception {
    makePageWithContent("TestPageOne", "test page");
  
    submitRequest("TestPageOne", "type:data");
  
    assertResponseIsXML();
    assertResponseContains("test page", "<Test");
  }
  ```

### 이중 표준

- 테스트 코드에 적용하는 표준은 실제 코드에 적용하는 표준과 다르다.

  - 실제 코드만큼 효율적일 필요가 없다.
  - 실제 환경과 테스트 환경은 요구사항이 다르다.
    - 테스트 환경은 자원이 제한적일 가능성이 낮다.

- AS-IS

  ```java
  @Test
  public void turnOnLoTempAlarmAtThreashold() throws Exception {
    hw.setTemp(WAY_TOO_COLD); 
    controller.tic(); 
    assertTrue(hw.heaterState());   
    assertTrue(hw.blowerState()); 
    assertFalse(hw.coolerState()); 
    assertFalse(hw.hiTempAlarm());       
    assertTrue(hw.loTempAlarm());
  }
  ```

- TO-BE

  ```java
  @Test
  public void turnOnLoTempAlarmAtThreshold() throws Exception {
    wayTooCold();
    assertEquals("HBchL", hw.getState()); 
  }
  ```

### 테스트 당 assert 하나

assert

- 테스트에 넣을 수 있는 정적 메서드 호출
- 어떤 조건이 참인지 거짓인지 검증하는 방법



**Given-When-Then Pattern**

- **Given** : 테스트에서 구체화하고자 하는 행동을 시작하기 전에 테스트 상태를 설명
- **When** : 구체화하고자 하는 행동
- **Then** : 파트는 어떤 특정한 행동 때문에 발생할거라고 예상되는 변화에 대해 설명

```
기능 : 사용자 주식 트레이드

시나리오 : 트레이드가 마감되기 전에 사용자가 판매를 요청
  
"Given" 나는 MSFT 주식을 100가지고 있다. 
        그리고 나는 APPL 주식을 150가지고 있다. 
		그리고 시간은 트레이드가 종료되기 전이다.

"When"  나는 MSFT 주식 20을 팔도록 요청했다.
     
"Then"  나는 MSFT 주식 80 가지고 있어야 한다.
		그리고 나는 APPL 주식 150을 가지고 있어야 한다.
		그리고 MSFT 주식 20이 판매 요청이 실행되었어야 한다.
public void testGetPageHierarchyAsXml() throws Exception { 
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
  
  whenRequestIsIssued("root", "type:pages");
  
  thenResponseShouldBeXML(); 
}

public void testGetPageHierarchyHasRightTags() throws Exception { 
  givenPages("PageOne", "PageOne.ChildOne", "PageTwo");
  
  whenRequestIsIssued("root", "type:pages");
  
  thenResponseShouldContain(
    "<name>PageOne</name>", "<name>PageTwo</name>", "<name>ChildOne</name>"
  ); 
}
```

**Template Method Pattern**

- 알고리즘의 구조를 메소드에 정의하고, 하위 클래스에서 알고리즘 구조의 변경없이 알고리즘을 재정의 하는 패턴
- 알고리즘이 단계별로 나누어 지거나, 같은 역할을 하는 메소드
- 여러곳에서 다른형태로 사용이 필요한 경우 유용한 패턴

→ 중복을 제거할 수 있지만, 공통 부분을 별도의 method로 만드는데 공수가 더 클 수 있다.

단일 assert 문이 훌륭하다고 생각하지만, 여러 assert를 써야할 필요가 있다.

- 단지 assert 문 개수를 최대한 줄이자.

### 테스트 당 개념 하나

- 테스트 함수마다 하나의 개념만 테스트하자
- 여러 개념을 한 함수로 몰아넣으면 개발자가 개념을 모두 이해하는 것이 어렵다.

```java
/**
 * addMonth() 메서드를 테스트하는 장황한 코드
 */
public void testAddMonths() {
	SerialDate d1 = SerialDate.createInstance(31, 5, 2004);
	
	// (6월처럼) 30일로 끝나는 한 달을 더하면 날짜는 30일이 되어야지 31일이 되어서는 안된다.
	SerialDate d2 = SerialDate.addMonths(1, d1); 
	assertEquals(30, d2.getDayOfMonth()); 
	assertEquals(6, d2.getMonth()); 
	assertEquals(2004, d2.getYYYY());

	// 두 달을 더하면 그리고 두 번째 달이 31일로 끝나면 날짜는 31일이 되어야 한다.
	SerialDate d3 = SerialDate.addMonths(2, d1); 
	assertEquals(31, d3.getDayOfMonth()); 
	assertEquals(7, d3.getMonth()); 
	assertEquals(2004, d3.getYYYY());

	// 31일로 끝나는 한 달을 더하면 날짜는 30일이 되어야지 31일이 되어서는 안된다.
	SerialDate d4 = SerialDate.addMonths(1, SerialDate.addMonths(1, d1)); 
	assertEquals(30, d4.getDayOfMonth());
	assertEquals(7, d4.getMonth());
	assertEquals(2004, d4.getYYYY());
}
```

### F.I.R.S.T 법칙

- Fast (빠르게)
  - 테스트는 빨라야한다.
  - 코드를 돌릴 때 느리면 테스트 코드를 많이 못돌린다 → 코드 정리하기 힘들고 문제를 찾기 어렵다
- Independent (독립적으로)
  - 각 테스트는 서로 의존하면 안된다.
  - 테스트가 서로에게 의존하면 하나가 실패할 때 나머지도 잇달아 실패 → 원인을 진단하기 힘들다.
- Repeatable (반복 가능하게)
  - 어떤 환경에서도 반복이 가능해야한다.
  - 환경이 지원하지 않으면 테스트를 수행하지 못하는 상황에 직면한다.
- Self-Vaildating (자가 검증하는)
  - 테스트는 bool 값으로 결과를 내야한다. (성공 / 실패)
  - 테스트가 스스로 성공과 실패를 가늠하지 않는다면 판단은 주관적이게 되고 수동 평가가 되어버린다.
- Timely (적시에)
  - 테스트 코드는 적시에 작성해야한다.
  - 테스트를 실제 코드를 구현하기 직전에 구현한다.
    - 실제 코드를 구현한 이후에 테스트 코드를 짜면, 실제 코드가 테스트 하기 어렵다고 판단할 수 있다.