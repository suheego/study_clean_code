# Chapter. 12 창발성

잘 골랐다... 짧고 강렬...

![](https://images.velog.io/images/noahshin__11/post/36175030-0063-40fa-87e0-3ed0a820509b/image.png)

켄트 벡 왈!

다음 규칙을 따르면 설계는 '단순하다'고 말한다. ( 맨위가 제일 중요 그리고 아래로)

- 모든 테스트를 실행한다
- 중복을 없앤다
- 프로그래머 의도를 표현한다
- 클래스와 메서드 수를 최소로 줄인다.

## 1. 모든 테스트를 실행하라

또 TDD? 또? 또디디 또DD 진짜...나도 하고 싶다고... 아직 CTO가 TDD 만들 시간 없다는데. 우떡해..

- 테스트가 가능한 시스템을 만들려고 애쓰면 설계 품질이 더불어 높아진다
- 크기가 작고 목적하나만 수행하는 클래스가 나온다.
- SRP를 준수하는 클래스는 테스트가 훨씬 더 쉽다. (single responseibility Principle 단일 책임원칙)
- 테스트 케이스가 많을수록 개발자는 테스트가 쉽게 코드를 작성한다.
- 철저한 테스트가 가능한 시스템을 만들면 더 나은 설계가 얻어진다

노올랍게도... "테스트 케이스를 만들고 계속 돌려라" 라는 간단하고 단순한 규칙을 따르면 시스템은 낮은 결합도와 높은 응집력이라는, 객체 지향 방법론이 지향하는 목표를 저절로 달성 한다.

결론: 응 테스트 코드 짜~

## 리팩터링

리팩터링을 하다가 기존기능의 시스템이나 기능이 깨질까봐 걱정하는가?

걱정하쥐 마롸~! 우리에겐 테스트 케이스가 있으니까! 

- 응집도를 높이고,
- 결합도를 낮추고
- 관심사를 모듈로 분리하고
- 함수와 클래스 크기 줄이고
- 더 나은 이름을 선택하고...

## 중복을 없애라 !!

공통적인 코드가 있다면 메서드로 만들자

깔끔한 시스템을 만들려면 단 몇줄이라도 중복을 제거하겠다는 의지가 필요하다.

## 표현하라 !!

자신이 이해하는 코드는 짜기 쉽다. 내가 짰으니까 ! 

짤때는 그 로직에 푹 빠져서 코드를 구석구석 이해하지. 

하지만 나중에 코드를 유지보수할 사람이 코드를 짜는 사람은.. ㅎ ㅏ... 거의 수능 국어 글쓴이 마음 헤아리기 (정작 본인은 수능 본 적 없음)

소프트웨어 프로젝트 비용 중 대다수는 장기적인 유지보수에 들어간다.

그래서 코드는 개발자의 의도를 분명히 표현해야 한다. 

- 좋은 이름 !
- 함수와 클래스 크기를 가능한 줄인다.
- 표준 명칭을 사용한다. 회사에서 쓰는 명칭을 고수해라 (user , customer, consumer, people 다 비슷한데 ~~호갱노노~~ 혼용 노노)
- 유닛 테스트 잘 짜라

## 클래스와 메서드 수를 최소로 줄여라

목표는 함수와 클래스 크기를 작게 유지하면서 동시에 시스템 크기도 작게 유지하는 데 있다. 하지만 이 규칙은 간단한 설계 규칙 네 개 중 우선순위가 낮다. 

 다시말해, 클래스와 함수 수를 줄이는 작업도 중요하지만, 테스트 케이스를 만들고 중복을 제거하고 의도를 표현하는 작업이 더 중요.

# 결론:

마 이게 바이블이다~ 생각하고 따르라!
