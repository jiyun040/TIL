> InheritedWidget과 비슷한 동작을 하는 새로운 매커니즘으로 재구현한 상태 관리 패키지
> 
*InheritedWidget: Tree의 Top에 접근해 바로 데이터를 가져올 수 있도록 하는 위젯

- 공식사이트: 리액티브 캐싱, 데이터 바인딩 프레임 워크이다.

**- 리액티브 캐싱 ( Reactive Caching )**

- 데이터를 비동기적으로 계산할 때 캐싱해 해당 데이터가 필요한 모든 곳에서 쉽게 접근 할 수 있도록 하는 것
- Riverpod 에서는 Provider를 이용해 상태를 지속적으로 캐시하고 노출함으로써 데이터를 쉽게 공유하고 동기화할 수 있다.
  ⇒ 처음 데이터를 가져온 뒤에는 동일한 데이터를 다른 컴포넌트에서 다시 가져올 필요 없이 이미 캐시된 데이터를 활용해 중복 호출을 줄인다.

**- 데이터 바인딩 ( Data Binding )**

- UI + 데이터 → 데이터의 변경에 따라 UI를 자동으로 변경한다.
- Riverpod 이용시 상태를 나타내는 Provider 및 ref 객체의 watch 메서드를 통해 상태의 변경을 관찰해 상태가 변경되면 UI가 자동으로 반영할 수 있도록 한다. 
  ⇒ MVVM 패턴 구현 가능하다.

*MVVM ( Model View, View, Model ) 패턴: 비즈니스 로직과 프레젠테이션 로직을 UI로 명확하게 분리하는 패턴

Riverpod는 Provider 패키지의 상위 버전 패키지다. 버전업을 하지 않고 새로 배포한 이유는 Provider의 결함을 수정하기 위해선 매커니즘을 크게 수정할 수 있을뿐 이였기 떄문이다. 

**- Provider의 대표적인 결함**

- 런타임에 ProviderNotFoundException이 호출되는 문제
- Provider를 더 이상 사용하지 않을 때에는 수동으로 dispose를 해줘야 하는 문제
- 다른 Provider를 의존하는 복잡한 Provider를 생성할 수 없는 문제

**- Riverpod의 장점**

- Provider의 장점
    - 위젯 리빌드 시 상태 손실 걱정 없이 create, observe, dispose 할 수 있다.
    - lazy loading을 지원해 사용자가 사용중이지 않은 Provider를 당장 로딩하지 않는다..
    - 플러터의 devetool에서 Provider 객체 관찰이 가능하다.
    - 단방향의 데이터 흐름으로 앱의 확장성을 높일 수 있다.
- Riverpod의 장점
    - Riverpod는 Compile-safe 하다.
      ⇒ 사용에 문제가 있다면 컴파일 타임에 발견하므로 더이상 런타임에 ProviderNotFoundException이 발생하지 않는다.
    - Flutter SDK에 의존하지 않는다.
      ⇒ Provider에서는 동일한 타입의 Provider를 만들 수 없어서 동일한 타입을 정의하려면 커스텀 클래스를 정의해야만 했었다.
    - 최소한의 보일러 플레이트 코드로 다른 Provider와 간단하게 결합할 수 있다.
      ⇒ Provider에서는 여러 Provider를 결합할 경우 다른 형태인 ProxyProvider를 사용해야 한다.
    - 테스트가 간편해진다.
      ⇒ 테스트 내에서 Provider를 재정의 할 필요 없고 setUp / tearDown 단계가 불필요해져 특정 행위에 대한 테스트가 쉬워진다.
    - 범위 지정에 대한 과도한 의존도를 중리고 사용되지 않는 Provider의 상태를 자동으로 dispose 시킬 수 있다.

- ProviderScope

앱 전체를 `ProviderScope`로 감싸야 **Riverpod가 사용** 가능하다.

- ref 객체

Provider를 **읽기 위해** 필요하다.

ref객체는 StatelessWidget에서는 얻을 수 없고 ConsumerWidget을 사용하거나 사용하고 싶은 위젯의 부분에서 Consumer 위젯으로 감싸면 ref 객체를 사용할 수 있게된다. 

*ConsumerWidget: Provider 객체의 상태를 구독하고, 상태의 변경이 있을 때 마다 build를 호출해 화면을 다시 그린다. 

- watch 메서드

Provider의 값을 구독해 **가져올 때** 사용한다. 

**사용예제**

1. provider.dart

```dart
import 'package:flutter_riverpod/flutter_riverpod.dart';

// 정수형 provider 선언
final numberProvider = Provider<int>((ref) {
	return 42;
});
```

1. main.dart

```dart
import 'package:flutter/material.dart';
import 'package:flutter_riverpod/flutter_riverpod.dart';
import 'provider.dart';

void main() {
	runApp(
		// 전체를 ProviderScope로 감싸야 Riverpod 사용 가능
		const ProviderScope(child: MyApp()),
	);
}

class MyApp extends StatelessWidget {
	const MyApp({super.key});
	
	@override
	Widget build(BuildContext context) {
		return const MaterialApp(
			home: HomePage(),
		);
	}
}
```

1. HomePage에서 Provider 사용

```dart
class HomePage extends ConsumerWidget {
	const HomePage({super.key});
	
	@override
	Widget build(BuildContext context, WidgetRef ref) {
		// ref를 이용해 provider의 값 읽기
		final number = ref.watch(numberProvider);
		
		return Scaffold(
			body: Center(
				child: Text(
					'Provider 값: $number'
				),
			),
		);
	}
}
```
