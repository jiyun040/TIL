어플을 사용할 때 하단에 페이지를 이동할 수 있는 메뉴들이 있다. 

그런 것들을 사용하기 위해서는 `BottomNavigationBar`라는 위젯을 사용하면 된다. 

```dart
class App extends StatefulWidget {
	...
}

class _AppState extends State<App> {
	...
	return Scaffold(
		...
		bottomNavigationBar: BottomNavirationBar(
			items: [
				BottomNavigationBarItem(icon: Icon(Icons.home), label: 'home'),
				BottomNavigationBarItem(icon: Icon(Icons.account_circle_rounded), label: 'mypage'),
				BottomNavigatoinBarItem(icon: Icon(Icons.menu), label: 'menu'),
			],),
		);
	}
}
```

Scaffold 안에는 bottomNavigationBar라는 프로퍼티가 존재한다.

BottomNavigationBar 말고 커스텀을 이용해 bottomAppBar로 만든 것도 전달이 가능하다. 

`BottomNavigationBar`에는 items 프로퍼티를 무조건 정의해야 오류가 발생하지 않는다. 

items는 List타입을 기본적으로 전달받으며 제너릭으로 BottomNavigationBarItem이 정의되어 있다.

현재 페이지를 리다이렉트 할 때 즉, 다른 페이지로 이동 할 때 items의 인덱스를 통해서 관리할 수 있다. 

items의 리스트에서 특정 인덱스를 선택하면 해당 인덱스에 해당하는 페이지로 이동하도록 관리 할 수 있다는 뜻 이다. 

그렇게 하기 위한 프로퍼티가 currentIndex이다. 

```dart
...
class _AppState extends State<App> {
	var _index = 0;
	
	@override
	Widget build(BuildContext context) {
		return Scaffold(
		...,
		bottomNavigationBar: Bottom NavigationBar(
			currentIndex: _index, 
			items: [
				...
```

**home, mypage, menu**

여기서 만들 코드 셋 다 내부는 같은데 클래스 이름과 내부에 표시하는 글자만 다르다. 

```dart
// home
class Home extends StatelessWidget {
	const Home({super.ley});
	
	@override
	Widget build(BuildContext context) {
		return Container(
			child: Center(child: Text('home'),),
		);
	}
}
```

```dart
// App
class App extends StatefulWidget {
	...
}

class _AppState extends State<App> {
	var _index = 0;

	List<Widget> _pages = [
		Home(),
		MyPage(),
		Menu(),
	];
	
	@override
	Widget build(BuildContext context) {
		return Scaffold(
			body: _pages[_index],
			bottomNavigationBar: BottomNavigationBar(
				currentIndex: _index,
				onTap: (value) {
					setState(() {
						_index = value;
						print(_index);
					});
				},
				items: [
					BottomNavigationBarItem(icon: Icon(Icons.home), label: 'home'),
					BottomNavigationBarItem(icon: Icon(Icons.account_circle_rounded), label: 'mypage'),
					BottomNavigationBarItem(icon: Icon(Icons.menu), label: 'menu'),
					],
				),
			);
		}
	}
```

잘 해주었다면 각각의 home, mypage, menu를 눌렀을 때 그 페이지로 이동하게된다. 

**그 외의 옵션**

```dart
showSelectedLabel: ,
showUnselectedLabels: ,
// 둘 다 flase로 주게 되면 밑의 글씨들을 없앨 수 있다. 
```

```dart
BottomNavigationBarItem(..., activeIcons: Icon(Icons.star)),
// 그 아이콘을 활성화했을 때 바뀌는 아이콘 지정
```
