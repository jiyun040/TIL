Navigator는 기본적으로 MaterialApp에 포함된 Root Navigator가 있다.

이 Root Navigator는 앱 전체의 페이지 스택을 관리한다.

MainPage ( Page1, Page2 )에서 Page3으로 넘어갈 때 `Navigator.pop`을 사용하게 되면 전체페이지가 바뀌게 될 것이다. 

그러나 Nested Navigator를 사용하게 되면 MainPage의 기본적인 것은 유지되고 화면만 바뀌게 될 것 이다. 

```dart
// Navigator를 식별하는 key
final _navigatorKey = GlobalKey<NavigatorState>();

// GlobalKey로 식별한 Nested Navigator를 활용해 화면 전환
void _onComplete() {
	_navigatorKey.currentState!.pushNamed(routeDeviceSetupSelectDevicePage);
}

void _onSelected(String devideId) {
	_navigatorKey.currentState!.pushNamed(routeDeviceSetupConnectingPage);
}

void _oNConnection() {
	_navigatorKey.currentState!.pushNamed(routeDeviceSetupFinishedPage);
}

@override
Widget build(BuildContect context) {
	return WillPopScope(
		onWillPop: _isExitDesired,
		child: Scaffold(
			// AppBar 고정
			appBar: _buildAlowAppBar(),
			// body에 Navigator를 할당해 이 부분만 전환이 일어나도록
			body: Navigator(
				// Navigator를 식별하기 위한 key 할당
				key: _navigatorKey,
				initialRoute: widget.setupPageRoute,
				onGenerateRoute: _onGenerateRoute,
			),
		),
	);
}

// push할 때 RouteSettings에 따라 Route 생성
Route _onGenerateRoute(RouteSettings settings) {
  late Widget page;
  switch (settings.name) {
    case routeDeviceSetupStartPage:
      page = WaitingPage(
        message: 'Searching for nearby bulb...',
        onWaitComplete: _onDiscoveryComplete,
      );
      break;
    case routeDeviceSetupSelectDevicePage:
      page = SelectDevicePage(
        onDeviceSelected: _onDeviceSelected,
      );
      break;
    case routeDeviceSetupConnectingPage:
      page = WaitingPage(
        message: 'Connecting...',
        onWaitComplete: _onConnectionEstablished,
      );
      break;
    case routeDeviceSetupFinishedPage:
      page = FinishedPage(
        onFinishPressed: _exitSetup,
      );
      break;
  }
  
  // MaterialPageRoute에 담아서 리턴
  return MaterialPageRoute<dynamic>(
    builder: (context) {
      return page;
    },
    settings: settings,
  );
}
```

- `_navigatorKey`를 통해 코드의 다른 부분에서 이 Nested Navigator를 참조하여 `pushNamed`, `pop` 등의 내비게이션 작업 수행 가능하다. 
- `_navigatorKey.currentState`: `_navigatorKey`에 연결된 `Navigator` 위젯의 현재 상태에 접근한다. 
- `pushNamed(...)`: 지정된 라우트 이름에 해당하는 새 화면을 현재 내비게이터 스택 위에 쌓아 올린다.
- `WillPopScope`**:**
    - 현재 화면을 벗어나려 할 때, 즉 뒤로가기를 눌렀을 때 이를 가로채서 특정 동작을 수행할 수 있게 해주는 위젯
    - `onWillPop**:** _isExitDesired`: `_isExitDesired` 함수가 `true`를 반환하면 뒤로 가기 동작이 허용, `false`를 반환하면 동작이 취소된다.
    ⇒ 사용자가 특정 조건을 만족해야만 화면을 벗어날 수 있도록 제어할 수 있다.
- `key: _navigatorKey`: 위에서 정의한 `_navigatorKey`를 여기에 할당해 `Navigator` 위젯이 해당 키로 식별되게 한다.
- `initialRoute: widget.setupPageRoute`: Nested Navigator가 처음 로드될 때 표시할 초기 라우트를 지정한다.
- `onGenerateRoute: _onGenerateRoute`: 이 `Navigator`가 새 라우트를 생성해야 할 때 호출될 콜백 함수를 지정한다. 실제로 화면에 표시될 위젯을 라우트 이름에 따라 결정하는 부분이다.

|  | **Nested Navigator** | **Root Navigator** |
| --- | --- | --- |
| AppBar 유지 | 가능 | 불가능, 매 화면마다 다시 그려야 함 |
| 재사용성 | 높음 | 낮음 |
| 흐름 제어 | 명확 (내부에서 push/pop) | 복잡 (전역 상태 관리 필요) |
| UX 일관성 | 높음 | 낮음 |
| 설계 구조 | 모듈화 쉬움 | 페이지 의존성 높음 |
