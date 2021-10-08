# Chapter 5 !

![](https://images.velog.io/images/noahshin__11/post/e150267b-3eee-464c-aea7-ae9e865a4cff/image.png)


## 형식을 맞추는 목적

- 코드의 형식은 중요하다 !
- 융통성 없이 맹목적으로 따르면 안 된다 !
- 의사소통은 전문 개발자의 1차적인 의무 !
- "돌아가는 코드" 가 1차적인 의무라 여길지도 모르지만 , 이제부터는 아니다 !
- 오늘 구현한 기능이 다음 버전에서 그대로 온전히 존재할리 만무하다 !

그럼 오또케?

## 적절한 행 길이를 유지하라

- 세로 길이부터
  - 파일마다 200줄 미만?
  - 길다고 좋은게 아니다
  - 반드시 지킬 엄격한 규칙은 아니지만 바람직한 규칙
  - 일반적으로 큰 파일보다 작은 파일이 이해하기 쉽다.
  - 노아: 솔직히 사바사 케바케 코바코이고,, 내가 아직 네카라쿠배 양반들의 귀하신 현업 소스코드를 본 적이 없고,, 지금 회사에서도 내가 소스코드를 다 맡고 있으니...적당한 세로길이란..?

![](https://images.velog.io/images/noahshin__11/post/e008dbb8-0513-49a9-9e26-968656acd5e2/image.png)


## 신문 기사처럼 작성하라


![](https://images.velog.io/images/noahshin__11/post/130e7b06-e938-4525-bcb0-93625d637cb8/image.png)
- 신문 독자는 위에서 아래로 읽고
  - 최상단의 기사를 몇 마디로 요약하는 표제가 나온다.
    - 첫 문단은 전체 기사 내용을 요약한다.
      - 쭉 읽으면 세세한 사실이 조금씩 드러난다. 날짜, 이름, 발언, 주장, 기타 세부사항이 나오지
- 이처럼 소스파일도 이름은 간단하면서 설명이 사능하게 !!
  - 이름만 보고도 올바른 모듈인지 아닌지 알 수 있도록,
  - 읽으며 내려갈수록 의도를 세세하게 묘사
  - 마지막에는 가장 저차원 함수와 세부 내역이 나온다.
- 신문은 다양한 기사로 이뤄진다
  - 대다수 기사가 짧다. 한 면을 채우는 기사는 거의 없다.

## 개념은 빈 행으로 분리하라

- 인간적으로 갓직히 함수 하나 끝나거나 IF 문 끝날 때 마다 엔터 쳐서 줄 나눔 해주는 거 기본 인정?
- 안 하고 잘 읽으면 알파고지...

## 세로 밀집도

```jsx
public class ReporterConfig {
 /** 
  * 리포터 리스너의 클래스 이름
  */
	private String m_className;

/** 
  * 리포터 리스너의 속성
  */
	private List<Property> m_properties = new ArraryList<Property>();
	public void addProperty(Property property) {
		m_properties.add(property);
	}
}
public class ReporterConfig {
	private String m_className;
	private List<Property> m_properties = new ArraryList<Property>();
	public void addProperty(Property property) {
		m_properties.add(property);
	}
}
```

주석으로 설명도 좋지만 아래 코드가 "한눈"에 읽기 더 쉽다.

척 보면 변수 2개에 메서드가 1개인 클래스라는 사실이 드러난다 ( 응 난 몰랐어)

## 수직 거리

- 변수 선언
  - 변수를 사용하는 위치에 최대한 가까이 선언한다.
  - (우리가 만든 함수는 매우 짧으므로 지역 변수는 각 함수 맨 처음에 선언한다)
- 인스턴스 변수
  - 반면, 인스턴스 변수는 클래스 맨 처음에 선언한다.
  - 잘 설계한 클래스는 많은 (혹은 대다수) 클래스 메서드가 인스턴스 변수를 사용하기 때문이다.

## 종속 함수

- 한 함수가 다른 함수를 호출한다면 두 함수는 세로로 가까이 배치한다.
- 또한 가능하다면 호출하는 함수를 호출되는 함수보다 먼저 배치한다.

좀 더 풀어서 설명하자면

- 첫째 함수에서 가장 먼저 호출하는 함수가 바로 아래 정의된다.
- 다음으로 호출하는 함수는 그 아래에 정의된다.
- 그러므로 호출되는 함수를 찾기가 쉬워지며, 그만큼 모듈 전체의 가독성도 높아진다.

## 개념적 유사성

- 어떤 코드는 개념적인 친화도가 높기 때문에 서로 끌어당긴다
- 친화도가 높을 수록 코드를 가까이 배치한다.

```java
if (imei === '') {
	return new BadRequestException(
		{ message_id: 'IMEI_SHOULD_NOT_BE_EMPTY',
      message   : 'IMEI should not be empty, please put something here' });
      }

if (serial_number === '') {
	return new BadRequestException(
		{ message_id: 'SERIAL_NUMBER_SHOULD_NOT_BE_EMPTY',
      message   : 'serial_number should not be empty, please put something here' });
      }
```

## 가로 형식 맞추기

- 80자 이하? 120자 이하?
- 갓직히 200자 안 넘으면 되고 이건 회바회

## 가로정렬

```java
			const messageId           = data.messageId;
      const machineId           = data.machineId;   // 20 digits
      const accountType         = data.accountType; // '11' Starbucks Korea App 12 HH
      const fullUserCode        = data.userCode;    // 50 digits
      const requestedTimeByUser = data.rewardRequestedTimeByUser;  //'2021-05-18 13:11:44'
      logger.info(`Message ID              = ${messageId}`);
      logger.info(`Machine ID              = ${machineId}`);
      logger.info(`Account Type            = ${accountType}`);
      logger.info(`Requested Time By User  = ${requestedTimeByUser}`);
```

음... 위코드에서 배운 컨밴션인데..

지금회사서도 잘 쓰고 있음

## 들여쓰기
![](https://i1.ruliweb.com/img/21/02/27/177e150151e5381f9.jpg)
- 파이썬 하던 시절에 Indent로 고생 한 적도 있지만 그만큼 깔끔했던게 없었는데...
- 자스 오니까 들여쓰기 지옥... 사실 eslint 와 프리티어가 자동으로 해주긴하는데,,음,,
- 뭐랄까... 너무 자유도가 떨어 진달까? 위처럼 가로정렬도 안댐..
- 그래서... 가독성이 좋게 알아서 하기로?
- 그리고 자동으로 하면 너무 쓸때없이 세로로 길어질 때가 있었는데
- 이것도 팀바팀, 회바회, 코바코

![](https://images.velog.io/images/noahshin__11/post/c4ce13c8-1de9-4f01-ab46-f275919e2054/image.png)

# 마치며..

사실 회사 개발팀의 컨밴션이 분명히 있을 것이며

지키는 것을 준수하되

그것은 고이면 안되고 새로운 의견을 받아들일 준비가 되어있어야한다.

그리도 가독성 있게 짜는거 사실 어려운거 아닌데 "귀찮아서", "바빠서" 잖아..

![](https://images.velog.io/images/noahshin__11/post/a002cebb-42e0-4daa-b834-11453708a721/Screen%20Shot%202021-10-08%20at%2010.16.36%20AM.png)

## 남을 위해 코딩하자 !