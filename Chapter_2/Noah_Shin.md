# Chapter 2. 의미 있는 이름

## 🌱 의도를 분명히 밝혀라

```jsx
public List<int[]> getThem() {
	List<int[]> list1 = new ArrayList<int[]>();
		for (int[] x : theList)
			if (x[0] == 4)
				list1.add(x);
		return list1;
}
```

1. theList에 무엇이 들었는가?
2. theList에서 0번째 값이 어째서 중요한가?
3. 값 4는 무슨 의미인가?
4. 함수가 반환하는 리스트 list1을 어떻게 사용하는가?

- 정보 제공은 충분히 가능했었다 !! 지뢰 찾기게임을 만든다고 가정하자.
  - 게임판에서 각 칸은 단순 배열로 표현
  - 배열에서 0번째 값은 칸 상태를 뜻한다. 값 4는 깃발이 꽃힌 상태
  - 각 개념에 이름만 붙여도 코드가 상당히 나아진다.

```jsx
public List<int[]> getFlaggedCelss() {
	List<int[]> flaggedCells = new ArrayList<int[]>();
		for (int[] cell : gameBoard)
			if (cell[STATUS_VALUE] == FLAGGED)
				flaggedCells.add(cell);
		return flaggedCells;
}
```


![Untitled1](https://media.vlpt.us/post-images/rajephon/c9322c30-5913-11e9-8a43-033dcf59c588/banner-image.jpg)

코드의 단순성은 변하지 않았다. 들여쓰기 단계도 동일, 하지만 코드는 더 명확해졌다.

코드의 단순성은 변하지 않았다. 들여쓰기 단계도 동일, 하지만 코드는 더 명확해졌다.

☢️  한 걸음 더 나아가, int 배열을 사용하는 대신, 칸을 간단한 클래스로 만들어도 되겠다. isFlaged 라는 좀더 명시적인 함수를 사용해 FLAGGED라는 상수를 감춰도 좋겠다.

```jsx
public List<Cell> getFlaggedCelss() {
	List<Cell> flaggedCells = new ArrayList<Cell>();
		for (Cell cell : gameBoard)
			if (cell.isFlagged())
				flaggedCells.add(cell);
		return flaggedCells;
}
```

노아: 이게 왜 더 나은지 솔직히 모르겠따. 다 클래스로 만들어 버리는게 좋은건가. 객체 지향?

## 🌱 발음하기 쉬운 이름을 사용하라

- 약어를 쓰지마라.

  - ex) genymdhms (generate date, year, month, day, hour, minute, second)
  - 회의할 때 "젠야므드흠드스"라고 불렀다고 한다. 🤮

  ![Untitled](https://t1.daumcdn.net/liveboard/interstella-story/461ea0b8ea884d6da9c3655af1e20d78.jpg)

  - 왜 줄이는지는 알겠지만, 그래도 형편없는 이름이다. 줄일바에 길지만 명확한게 낫고, 이름길이는 범위 크기에 비례해야한다

## 🌱 검색하기 쉬운 이름을 사용하라

- 검색할 변수나 메소드 혹은 클래스를 생각할 때 그 기능을 생각하고 떠오르는 단어여야한다.

## 🌱 자신의 기억력을 자랑하지 마라

- 타협해서, 루프에서 반복횟수를 세는 변수 i,j,k 는 괜찮다, 단 루프범위가 아주 작고 다른 이름과 충돌하지 않을 때만 괜찮다. 루프에서 반복 횟수 변수는 전통적으로 한 글자를 사용하기 때문이다. 그 외에는 적절하지 못하다.
- 똑똑한 프로그래머와 전문가 프로그래머 사이에서 나타나는 차이 = 전문가는 **명료함이 최고**라는 사실을 이해한다. 제발 남을 위해 코딩하자 (현재 회사의 코딩 규칙중 하나)
- 클래스 이름과 객체 이름은 명사나 명사구가 적합하다. 동사 노노
- 메서드 이름은 동사나 동사구가 적하다. getAccount, setAccount, isAccount

## 🌱 기발한 이름은 피하라

- 가끔 재밌는 메서드 이름을 쓰고 싶지만.. 꾹 참구 .. 제발 정적이고 명료한 이름으로..

![Untitled](https://storage.googleapis.com/jjalbot-jjals/2018/12/zchfKs5FD/zzal.png)

## 🌱 한 개념에 한 단어를 사용하라

- get & fetch & retrieve 다 가져오는 의미인데, 앵간하면 통일하는것이 좋다

## 🌱 의미 있는 맥락을 추가하라

스스로 의미가 분명한 이름들이 있겠지만 살짝 애매모호 한 이름들도 있다.

예) firstName, lastName, street, houseNumber, city, state, zipcode 라는 변수가 있다.

여기서 state는 미국에서 "주" 라는 의미로도 쓰이지만, status 같이 현황, 현상태 라는 의미로도 쓰일 수 있기 때문에 addr라는 접두어를 추가해,

- addrFirstName, addrStreet, addrState

☢️  개인적으로 addr 도 싫어한다. ess 세글자 줄여서 무슨 부귀영화를 누리는가. 개인적으로 현 회사에서는 최대한 안 줄이고 있다.

# 🌱 마치면서

좋은 이름을 선택하려면 설명 능력이 뛰어나야 하고 문화적인 배경이 같아야 한다. 이건 어렵다, 고로 여느 코드 개선 노력과 마찬가지로 이름 역시 나름대로 바꿨다가 누군가 질책할지도 모른다고 생각하지말고 코드를 개선하려는 노력을 중단해서는 안된다.

제발 암기는 요즘 도구에게 맡기고, 쉽고 명확한 이름을 사용하자.

- 갓직히, x , y << 이런 한 글자짜리 절때 안쓰고 (for 문 제외)
- 변수, 메쏘드, 클래스 이름 정할 때, 뭔가 애매하면 5분만 투자하면 나중에 리팩토링할때나 다시 검색할 때 10분 절약 가능함.
