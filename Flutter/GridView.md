### GridView

말 그대로 격자무늬처럼 보이기 위해 사용한다. 

그리드의 main축 방향은 스크롤 되는 방향이며 가장 일반적으로 쓰이는 그리드 레이아웃은 가로축에 고정된 수의 타일로 레이아웃을 만드는 `GridView.count`와 최대 가로축 범위를 갖는 타일로 레이아웃을 만드는 `GridView.extent`이다. 

커스텀 `SilverGridDelegrate`는 정렬되지 않거나 겹치는 배열을 포함해서 임의의 2D 자식 배열을 만들 수 있다. 

많거나 무한한 수의 자식이 있는 그리드를 만들기 위해서는 `GridView.builder`를 사용해서 GridDelegate에 `SilverGridDelegateWithFixedCrossAxisCount` 또는 `SilverGridDelegateWithFixedCrossAxisExtent` 중 하나를 사용한다. 

#### GridView.builder

- 동적으로 생성되는 항목에 적합하다.
- 필요할 때만 항목을 렌더링해서 성능을 최적화한다.

```dart
GridView.builder(
	itemCount; // item개수
	gridDelegate: SilverGridDelegrateWithFixedCrossAxisCount(
		crossAxisCount: , // 한개의 행에 보여줄 item 개수
		crossAxisSpacing: , // 열 간격
		mainAxisSpacing: , // 행 간격
	),
	itemBuilder: (BuildContext context, int index) {...} // item의 반복문
)
```

#### GridView.count

- 간단하게 고정된 열 수를 지정할 때 사용한다.

```dart
GridView.count(
	crossAxisCount: , // 한개의 행에 보여줄 item 개수
	crossAxisSpacing: , // 열 간격
	mainAxisSpacing: , // 행 간격
	
	childAspectRatio: 1/2, // item의 가로1, 세로2의 비율
	children: List.generate(length, (index) {...} // item의 반복문
	),
),
```

#### GridView.extent

- 각 항목의 최대 너비를 지정한다.
- 동적으로 열의 개수를 지정한다.

```dart
GridView.extent(
	mainAxisExtent: , // 각 항목의 최대 너비
	crossAxisSpacing: , // 열 간격
	mainAxisSpacing: , // 행 간격
	
	childAspectRatio: 1/2, // item의 가로1, 세로2의 비울
	children: List.generate(colorList, length, (index) {...} // item의 반복문
```

#### GridView.custom

- 고급 사용자 정의 레이아웃을 구현할 때 사용
- SilverGridDelegate를 커스텀 가능

```dart
GridView.custom(
	gridDelegate: SilverGridDelegateWithFixedCrossAxisCount(
		crossAxisCount: ,
		crossAxisSpacing: ,
		mainAxisSpacing: ,
	),
	
	childrenDelegate: SilverChildBuilderDelegate(...),
```

*SilverGridDelegateWithFixedCrossAxissCount

 : 고정된 열 개수를 사용한다.
   한 줄에 몇 개의 항목을 배치할지 지정한다.
   각 항목의 크기는 GridView의 크기와 열 개수에 따라 자동으로 정해진다.

```dart
SilverGridDelegateWithFixedCrossAxisCount(
	crossAxisCount: , // 한줄에 배치할 할목의 개수
	crossAxisSpacing: , // 열 간격
	mainAxisSpacing: , // 행 간격
```

만약 화면의 크기가 300이고 crossAxisCount가 3이라면 각 항목의 너비는 100으로 자동으로 설정된다. 

*SilverGridDlelgateWithFixedCrossAxisExtent

 : 각 항목의 고정된 너비 또는 높이를 사용한다.
   항목의 최대 크기를 기준으로 배치되며, 한 줄의 항목의 개수는 자동으로 계산한다.

```dart
SilverGridDelegateWithFixedCrossAxisExtent(
	maxCrossAxisExtent: , // 각 항목의 최대 너비
	crossAxisSpacing: , // 열 간격
	mainAxisSpacing: , // 행 간격
```

만약 항목의 최대 너비를 100으로 설정하면 화면의 크기가 300일 때 한 줄에 3개의 항목이 배치된다.
