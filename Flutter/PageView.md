여러 페이지를 한 화면에서  책을 넘기는 것과 같이 구현할 수 있도록 해주는 위젯 클래스이다. 

페이를 수직, 수평으로 넘기거나 페이지 전환 애니메이션 등을 적용할 수 있다. 

PageController 클래스를 만들어 페이지 전환을 트리거 할 수도 있다. (→ PageController를 사용해 PageView의 페이지를 프로그래밍적으로 변경하거나, 특정 페이지로 이동하는 것 등의 제어를 할 수 있다.)

- PageView 추가

```dart
PageView(
	children: [
		SixedBox.expand(
			child: Container(
				color: Colors.red,
					child: Center(
						child: Text(
							'Page Index : 0',
							style: Texstyle(fontSize: 20),
						),
					),
				),
			),
			SizedBox.expand(
				child: Container(
					color: Colors.yellow,
						child: Center(
							child: Text(
								'Page Index : 1',
								style: TextStyle(fontSize: 20),
							),
						),
					),
				),
				.
				.
				.
				.
```

- PageController 추가

```dart
final PageController pageController = PageController(
	initialPage: 0,
);

// 아까 만든 PageView
PageView(
	controller: pageController, // PageController와 연결
	children: [....
)
```

페이지 변경 시점에 애니메이션을 넣는다던다, 페이지를 넘기는 방향을 바꾼다는 등의 작업들을 통해 PageView를 커스텀 할 수 있다. 

- 페이지 넘기는 방향 변경

```dart
PageView(
	controller: pageController,
	scrollDirection: Axis.vertical, // 방향을 수직으로 설정
	childredn: [....
)
```

- 페이지 넘김 보정 끄기
페이지를 넘길 때, 화면이 페이지에 고정되도록 설정되어있는 것을 끌 수 있다.

```dart
PageView(
	controller: pageController, 
	pageSnapping: false, // 기능 끄기
	children: [...
)
```

---
ListView는 말 그대로 리스트 처럼 아래로 스크롤 하면서 볼 수 있었던 것 처럼

PageView도 말 그대로 Pgae처럼 넘기면서 볼 수 있는 것이다. 

ListView에 비해서는 비교적 간단 한 것 처럼 느껴진다. 
