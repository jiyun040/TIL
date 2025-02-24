### 1. MaterialApp 설정

Named route의 핵심은 route(스크린, 페이지, 하나의 위젯)들을 특정 이름으로 지정해 놓는 것이다. 이 이름을 지정하는 곳이 MaterialApp이다. 

initialRoute는 최초에 보여줄 route여서 반드시 설정해줘야 한다. 

routes에는 사용할 route들을 전달해줘야 한다. 

Map<String, WidgetBuilder>의 타입을 받고 있다. 여기서의 String은 routeName, WidgetBuilder는 이동할 스크린 위젯 이다. 

WidgetBuilder는 플러터 프레임워크에 사전에 정의된 타입으로 Context를 전달하고 Widget을 리턴하는 형태이다. 

밑의 코드에서는 2개의 route를 추가했다.

routeName “/”를 호출하면 RouteNamedSceren으로, “/first”를 호출하면 RouteFirst 화면으로 이동하게 된다. 

```dart
@override
Widger build(BuildContext context) {
	return MaterialApp(
		title: 'Flutter Named route',
		theme: ThemeData(primarySwatch: Colors.blue),
		dinitialRoute: "/",
		routes{
			"/": (context) => const RouteNamedScreen(),
			"/first": (context) => const ReouteFIrst(),
		},
	);
}
```

### 2. pushNamed

**RouteNamedScreen**

앱을 실행하면 최초에 진입하는 화면이다. 

코드를 보면 버튼이 하나 있고, 버튼을 클릭하면 Navigator.pushNamed 함수를 호출하고 있다. 

pushNamed에는 context와 이동할 routeName을 입력한다. 

```dart
class _RouteNamedScreenState extends State<RouteNamedScreen> {
String getData = "";

@override
Widget build(BuildContext context) {
	return Scaffold(
		body: Center(
			child: Column(
				mainAxisAlignment: MainAxisAlignment.center,
					children: [
						ElevatedButton(
							onPresed: () {
								Navigator.pushNamed(context, "/first?)'
							},
							child: const Text("Go first, pushNamed"),
						),
					],
				),
			),
		);
	}
}
```

**RouteFirst**

routeName을 “/first”로 지정해줬다. 

여기에는 뒤로 돌아가는 기능인 Navigator.pop을 호출하는 버튼이 있다. 

```dart
@override
	Widget build(BuildContext context) {
		return Scaffold(
			body: Center(
				child: Column(
					children: [
						ElevatedButton(
							onPressed: () {
								Navigator.pop(context);
							},
							child: Text("Pop")),
						],
					),
				),
			);
		}
```

### 3. pushReplacementNamed, pushNamedAndRemoveUntil

pushReplacementNamed의 사용법은 pushNamed와 동일하다. 

기능은 현재의 페이지를 새로 입력한 페이지로 교체하는 것 이다. 

```dart
ElevatedButton(
	onPressed: () {
		Navigator.pushReplacementNaemd(context, "/first");
	},
	child: const Text("pushReplacementNamed"),
),
```

pushNamedAndRemoveUntil은 pushAndRemoveUntil과 동일한 기능을 한다. 

특정 페이지로 이동하고 기존의 모든 페이지를 종료한다. 

```dart
ElevatedButton(
	onPressed: () {
		Navigator.pushNamedRemoveUntil(
			context,
			"/first:,
			(route) => false
		);
	},
	child: const Text("pushNamedAndRemoveUntil"),
),
```

### 4. argument 전달, 데이터 돌려받기

named route를 사용해서 데이터를 전달할 수도 있고, pop으로 돌아올 때 데이터를 돌려받을 수도 있다. 

**pushNamed 함수를 호출하면서 argument 전달**

아래 코드는 routeName을 입력한 후에 arguments 인자에 전달하고 싶은 데이터를 넣는다. 

String, int custom model 타입 모두 가능하다. 

pushNamed 함수의 return Type은Future<T?>이다. 

리턴 타입을 정하지 않아서 dynamic으로 했지만 직접 지정할 수도 있다. 

pop할 때 데이터를 보낸다면 result 변수에 값이 있게 오겠지만 데이터를 보내지 않는다면 null이 들어온다. 

```dart
ElevatedButton(
	 onPressed: () async {
		 dynamic result = await Navigator.of(context)
		  .pushNamed("/pushArgument", arguments: "pass argument");
		 setstate(() {
			 getData = result.toString();
			});
		},
		child: const Text("pushArgument"),
	)
```

```dart
// RoutePushArgument 페이지
@override
Widget build(BuildContext context) {
	final args = ModaRoute.of(context)!.settings.arguments;
	return Scaffold(
		body: Center(
			child: Column(
				mainAxisAlignment: MainAxisAlignment.center,
				children: [
					Text("argument: $args"),
					ElevatedButton(
						onPressed: () {
							Navigator.of(context).pop("데이터 전달");
						},
						child: const Text("페이지 종료 및 데이터 전달"))
					],
				),
			),
		);
	}
```

```dart
"/pushArgument": (context) => RoutePushArgument(),
// MaterialApp에 route 추가
```

---

named route를 사용했을 때의 편리한 것과 명확한 것이 있는데 특정 route만 제거 하는 것이 불가능 하다.

때문에 동일한 화면이 여러 개 존재할 수 있다는 것을 염두에 두고 사용해야겠다. 

그렇기에 route들을 다양하게 다뤄야 한다면 MaterialRoute를 사용하는 방법이 좋을 것 같다.

왜냐하면 화면 이동 시에 동일한 화면이 있다면 제거 후 이동할 수 있기 때문이다.
