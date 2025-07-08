#### 1. Route 와 Navigator

Navigator로 Route들을 관리한다.

- Route: screen 이나 page라고 불리는 위젯
- Navigator: Route(위젯)을 관리하는 위젯

#### 2. push, pop

push는 다른 페이지로 이동하는 것, pop은 이전 페이지로 이동하는 것을 의미한다. 

- push
사용하기 위해서는 context와 route가 필요하다.
route에는 MaterialPageRoute 클래스를 사용한다.
MaterialPageRoute는 이동하려는 Screen을 Route로 만들어준다.

```dart
Navigator.push(context, MaterialPageRoute(
	builder: (context) {
		return RouteSecondScreen();
	},
));
```

- pop
context만 넘겨주면 돼서 push에 비해 간단하다.

```dart
Navigator.pop(context);
```

#### 3. pushReplacement, pushAndRemoveUntil, popUntil

- pushReplacement
현재 페이지를 다른 페이지로 대체하는 함수이다. 
함수 이름은 다르지만 구현하는 형태는 push와 같다.

```dart
Navigator.pushReplacement(
	context,
	MaterialPageRoute(
		builder: (context) {
			return RouteSecondScreen();
		},
	),
);
```

- pushAndRemoveUntil
특정 화면으로 이동하고 기존에 있는 화면을 모두 제거한다.

```dart
Navigator.pushAndRemoveUntil(
	context,
	MaterialPageRoute(
		builder: (context) {
			return const HomeScreen();
		
		},
	),
	(route) -> false,
);
```

- popUntil
첫번째 라우트 까지 이동하고 그 외의 화면들은 종료한다.

```dart
Navigator.popUntil(
	context,
	(route) {
		return route.isFirst;;
	},
)
```

#### 4. removeRoute

특정 Route(스크린)를 지운다. 

하지만 Route Stack들을 관리 해줘야 하기에 구현하기에 생각보다 까다롭다. 

- Navigator.push 함수를 호출할 때 만든 MaterialRoute를 Map에 저장해 놓는다.
- removeRoute 함수를 호출할 때 Map 에 저장해 놓은 route가 있다면 removeRoute를 호출한다.

addRoute 함수에서 Route를 생성하고, 생성한 route를 routes에 저장한다. 

removeRoute 함수에서는 특정 스크린 이름으로 routes 변수에 추가된 것이 있다면 저장해 놓은 route를 불러와서 remove 한다. 

그러고 다시 routes의 값은 초기화 한다. 

**주요 기능: 화면 추가(addRoute), 화면 제거(removeRoute)**

```dart
Map<String, MaterialPageRoute<dynamic>?> routes = {
	'route_screen': null,
	'route_second_screen': null,
	'route_third_screen': null,
};

void addRoute({required String screen, required Function(MaterialPageRoute route) callback}) {
	MaterialPageRoute? route;
	switch (screen) {
		case 'route_screen':
			route = MaterialPageRoute(builder: (context) => const Routescreen());
			break;
		case 'route_second_screen':
			route = MaterialPageRoute(builder: (context) => const RouteSecondScreen(text: "stack"));
			break;
		case 'route_third_screen':
			route = MaterialPageRoute(builder: (context) => const RouteThirdScreen());
			break;
		}
		routes[screen] = route;
		callback(route!);
	}
	
	void removeRoute(BuildContext context, String screen) {
		if (routes[screen] != null) {
			Navigator.removeRoute(context, routes[screen]!);
			routes[screen] = null;
		}
	}
```
