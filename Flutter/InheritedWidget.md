### 필요한 이유

StatefulWIdget과 StatelessWidget을 통해 화면을 구성했을 때, 변화가 필요한 위젯이 트리의 끝에 있다면 트리의 top에서 bottom까지 불필요한 데이더 전달이 일어나게 된다. 

그래서 변화가 필요할 경우 트리를 통한 데이터를 받는 대신 바로 데이터를 가져올 수 있게 하는 것이 InheritedWidget이다. 

또, 상태관리 라이브러리인 Provider의 핵심 이기에 알아둬야 한다. 

자식 위젯에서 Inherited위젯을 어떻게 접근하냐하면 of 메서드를 사용하는 것이다. 

### 동작 원리

먼저 InheritedWidget을 extends 하는 class를 만든다. 

child 위젯은 반드시 포함해야하고 InheritedWidget인 ForgColor의 자손이 되는 모든 위//InheritedWidget을 상속한다. 젯은 해당 위젯의 필드에 접근할 수 있다. 

```dart
// InheritedWidget을 상속한다.
// 이 위젯을 상속받는다면 하위 위젯에서 데이터를 context를 통해 접근할 수 있다. 
class ForgColor extends InheritedWidget {
	const ForgColor({
		super.key,
		required this,color, required super.child,
	});
	
	final Color color;
	
	static ForgColor? maybeOf(BuildContext context) {
	// 밑의 코드를 사용해 트리에서 가장 가까운 ForgColor인스턴스를 검색한다. 
		return context.dependOnInheritedWidgetOfExactType<ForgColor>();
	}
	
	// of 함수를 정의한다. 
	static ForgColor of(BuildContext context) {
		final ForgColor? result = maybeOf(context);
		// null이라면 assert를 통해 오류를 발생시킴
		assert(result != null, 'No ForgColor found in context');
		return result;
	}
	
	//updateShouldNotify 함수를 재정의한다. 
	//oldvalue와 newvalue를 비교해서 위젯이 다시 그려지도록 하는 value값을 지정해주는 메서드
	@override
	bool updaterShouldNotify(ForgColor oldWidget) => color != oldWidget.color;
}
```

InheritedWidget의 예로는 Scaffold, Theme, FocusScope가 있다.
