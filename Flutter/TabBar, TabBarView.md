인스타 외의 다른 앱에서도 볼 수 있듯이 상단에서 넘기면서 해당 페이지로 이동하는 것이다.

### TabBar

TabController와 List<Widget> 인 tabs를 필수로 받는다. 

```dart
TabBar(
	controller: _tabController,
	tabs: [
		Tab(text: 'First'),
		Tab(text: 'Second'),
		Tab(text: 'Third'),
	],
)
```

#### **속성**

1. **Indicator**
현재 선택된 탭의 위치를 나타내는 indicator를 꾸미는 것들은 아래와 같다. 

```dart
// indicator의 색상 설정
indicatorColor: Colors.black,

// indicator 색상 자동 설정 여부
automaticIndicatorColorAdjustment: true,

// indicator의 굵기 설정
indicatorWeight: 2.0,

// indiactor의 padding 설정
indicatorPadding: EdgeInsets.zero,

// indicator 설정
indicator; BoxDecoration(),

// indicator의 크기 설정
indicatorSize: 10,
```

1. **label, unSelectedLabel**
label은 TabBar 내부에 있는 텍스트이다. 
unSelectedLabel을 설정하면 label은 selectedLabel과 동일하게 나타난다.
만약 unselectedLabelColor를 설정하면 현재 선택된 탭에만 labelColor가 적용된다. 
unselectedLabel을 설정하면 모든 탭의 텍스트가 동일하게 보인다. 

```dart
// label 색상 설정
// unselectedLabel이 설정ㄷ괴면 현재 선택된 탭의 label 색상
labelColor: Colors.black,

// label 글자 설정
// unselectedLabel이 설정되면 현재 선택된 탭의 label 글자
labelStyle: TextStyle(),
	
// label padding 설정
labelPadding: EdgeInsets.zero,
	
// 선택되지 않은 탭의 label 색상 설정
unselectedLabelColor: Colors.red,
	
// 선택되지 않은 탭의 label 글자 설정
unselectedLabelStyle: TextStyle(),
```

1. **isScrollable**
true로 설정하면 탭의 크기가 label의 크기만큼 할당 되어 스크롤이 가능해지며, 탭의 갯수가 많을 때 유용하다. 
반대로 false로 설정하면 탭의 크기가 1/n로 할당이 되어, 탭의 갯수가 적을 때 유용하다. 

```dart
isScrollable: false,
```

1. **overlayColor, splashFactory, splashBorderRadius**
overlayColor는 탭을 터치했을 때 나오는 색이다. 

```dart
overlayColor: MaterialStateProperty.resolveWith<Color?>(
	(Set<MaterialState> states) {
		return states.contains(MaterialState.focused)
			? null
			: Colors.red;
	},
),
```

        탭을 눌렀을 때 나타나는 동그란 점의 모양을 정의한다. 
        NoSplash.splashFactory로 동그란 점 효과를 없앨 수 있다. 

```dart
splashFactory: NoSplash.splashFactory,
```

        탭을 눌렀을 때 효과의 테두리를 설정한다. 

```dart
splashBorderRadius: BorderRadius.circular(10),
```

1. **기타**

```dart
// 탭 클릭 이벤트
// 현재 index 반환
onTap: (int index) {

},

// DragStartBehavior는 start로 설정하면 드래그 애니메이션이 더 부드러워지고
// down으로 설정하면 드래그 동작이 터치하는 순간 시작된다.
// start는 의도적으로 드래그 하는 느낌을 강조하고, down은 약간의 움직임도 감지
dragStartBehavior: DragStartBehavior.start,

// TabBar의 스크롤 방식을 지정
physics: BouncingScrollPhysics(),

// TabBar의 터치 효과음 여부
enableFeedback: false,

// Tab의 padding 설정
pdding: EdgeInsets.zero,
```

### TabBarView

좌우 손동작으로 화면을 이동시킬 수 있는 위젯이며 TabController와 children을 필수로 받는다. 

#### 속성

1. **viewportFraction**
TabBarView 내부에 있는 위젯이 화면을 차지하는 비율을 나타낸다. 
0. ~ 1.0 까지 지정 가능하다. 

```dart
viewportFraction: 1.0,
```

1. **physics**
TabBarView의 스크롤 방식을 설정한다. 
NeverScrollableScrollPhysics: 좌우 스크롤을 금지한다. 
BouncingScrollPhysics: 오버스크롤 효과가 튕겨나가는 효과로 바뀐다. 

```dart
physics: NeverScrollableScrollPhysics(),
```

1. **기타**

```dart
// DragStartBehavior는 start로 설정하면 드래그 애니메이션이 더 부드러워지고
// down으로 설정하면 드래그 동작이 터치하는 순간 시작된다.
// start는 의도적으로 드래그 하는 느낌을 강조하고, down은 약간의 움직임도 감지
dragStartBehavior: DragStartBehavior.start,
```

---

TabBar와 TabBarView를 함께 사용해 탭을 터치하거나 화면을 옆으로 넘겨서 전환할 수 있다. 

이걸 구현하기 위해서는 DefaultTabController를 사용 해 두개를 연결할 수 있다. 

TabController도 사용할 수 있지만 이걸 사용하면 TabController응 직접 생성하고 관리해야 한다. 

만약 탭 개수가 정해져 있고 기본적인 기능만 있어도 괜찮다면 `DefaultTabController`,
탭 이동을 프로그래밍적으로 제어하거나 커스텀 통작이 필요하다면 `TabController`를 직접 사용하는게 더 좋다. 

**예시코드**

```dart
class MyTabScreen extends StatelessWidget {
	@override
	Widget build(BuildContext context) {
		return DefaultTabController(
			length: 3, // 탭 개수
			child: Scaffold(
				appBar: AppBar(
					title: Text('탭 예제'),
					bottom: TabBar(
						tabs: [
							Tab(text: '홈'),
							Tab(text: '검색'),
							Tab(text: '설정'),
						],
					),
				),
				body: TabBarView(
					children: [
						Center(child: Text('홈 화면')),
						Center(child: Text('검색 화면')),
						Center(child: Text('설정 화면')),
					],
				),
			),
		);
	}
}
```
