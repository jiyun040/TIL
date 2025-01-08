Provider 패키지를 기반으로 구축된 라이브러리

Provider의 주요 단점을 해결하고 보다 유연하고 안전한 상태 관리를 제공

반응형 패러다임을 제공하며, 개발 중 발생할 수 있는 오류를 런타임이 아닌 컴파일 타임에 잡아낼 수 있다. 

**특징**

- 다음과 같은 핵심 개념을 기반으로 한다.
    - Providers: 데이터 소스를 선언한다.
    - Consumers: Provider를 읽고 변경 사항에 따라 재구성한다.
    - Notifiers: Provider를 업데이트 한다.
    - Riverpod는 내부적으로 스트림을 사용해서 provider가 변경될 때 consumer를 업데이트 한다.
- Riverpod의 일부 타입 안전성 문제를 개선했고, 상태 객체를 더 쉽게 테스트할 수 있다.
- 유연성과 범용성이 높다.

**작동 흐름**

1. 데이터를 위한 Provider Repository를 선언한다.
2. 앱을 Provider 컨테이너로 감싼다.
3. 컴포넌트는 consumer를 사용해서 provider에 접근한다. 
4. notifier를 통해 provider와 상호작용한다.
5. consumer는 provider의 변경에 따라 UI를 재구성한다.

**장점**

- Provider의 타입 안전성 문제를 개선했고, 더 안정적인 개발 환경을 제공한다.
- 상태 관리 로직을 쉽게 테스트하고 유지보수 할 수 있다.
- 거의 모든 종류의 상태 관리 시나리오에 적용할 수 있고, Provider를 사용하는 것보다 더 다양한 기능을 지원한다.

**단점**

- 새로운 개념과 패턴이 많아 처음 접한다면 러닝 커브가 높다고 느껴질 수 있다.
- 기존의 Provider 사용자가 Riverpod의 새로운 패러다임에 적응하는 데 시간이 걸릴 수 있다.

```Flutter
// State Notifier
class CounterNotifier extends StateNotifier<int> { //상태 관리 클래스
  CounterNotifier() : super(0); //초기 상태를 0으로 설정

  void increment() => state++; //1증가
  void decrement() => state--; //1감소
}

// Provider
final counterProvider = StateNotifierProvider<CounterNotifier, int>((ref) { //ref: Provider간의 의존성을 관리
  return CounterNotifier(); //상태 제공하는 역할
});

// Counter Widget using Riverpod
class SimpleCounter extends ConsumerWidget { //사용자가 상태 보고 버튼을 눌러 상태 변경할 수 있는 UI
  @override
  Widget build(BuildContext context, WidgetRef ref) {
    final count = ref.watch(counterProvider); //counterProvider의 상태를 감시, 상태가 변경되면 UI를 새로 그림
    return Column(
      children: [
        IconButton(
          icon: Icon(Icons.remove),
          onPressed: () => ref.read(counterProvider.notifier).decrement(), //counterProvider에 연결된 CounterNotifier 객체를 가져옴
        ),
        Text('Count: $count'),
        IconButton(
          icon: Icon(Icons.add),
          onPressed: () => ref.read(counterProvider.notifier).increment(),
        ),
      ],
    );
  }
}
```
