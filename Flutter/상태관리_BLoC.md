이벤트 기반의 상태 관리를 제공

에플리케이션의 비즈니스 로직을 UI로부터 분리 → 테스트가 용이, 확장성이 보다 높은 앱 구조를 만들 수 있다. 

**특징**

- 자체 컴포넌트에서 상태를 관리하며 UI와 비즈니스 로직의 철저한 분리를 통해 코드의 가독성과 유지 보수성을 향상시킨다.
- stream/sync와 반응형 프로그래밍을 사용한다.
- 데이터가 단방향으로 흐른다.

**작동 흐름**

1. UI 컴포넌트가 BLoC에 액션을 전달한다.
2. BLoC이 액션을 처리한다.
3. BLoC이 새로운 상태로 스트림을 업데이트 한다. 
4. StreamBuilder가 상태 변경을 감지한다.
5. 상태 변경에 따라 UI는 자동으로 재구성된다. 

**장점**

- UI로부터 비즈니스 로직을 완벽하게 분리하여 유지보수와 테스트가 용이하다.
- 확장성이 높고 복잡한 데이터 흐름과 상태를 관리하기 좋다.
- test-driven development에 적합하다.

작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복하여 구현한다. 

**단점**

- 간단한 애플리케이션에는 과도하게 복잡할 수도 있고, 설정과 사용에 많은 러닝 커브를 요구한다.
- 많은 보일러플레이트 코드를 필요로한다.

```dart
// Events
abstract class CounterEvent {}
class Increase extends CounterEvent {} //숫자 증가시키는 이벤트
class Decrease extends CounterEvent {} //숫자 감소시키는 이벤트

// BLoC, 이벤트를 받아서 상태를 바꿈
class CounterBloc extends Bloc<CounterEvent, int> {
  CounterBloc() : super(0); //초기 상태가 0

  @override
  Stream<int> mapEventToState(CounterEvent event) async* {
    if (event is Increase) { //Increase 이벤트가 들어오면 상태+1
      yield state + 1;
    } else if (event is Decrease) { //Decrease 이벤트가 들어오면 상태-1
      yield state - 1;
    }
  }
}

class SimpleCounter extends StatelessWidget {
  @override
  Widget build(BuildContext context) { //BLoC과 UI  연결
    return BlocProvider( //BLoC을 UI에 전달
      create: (_) => CounterBloc(), //BLoC 생성
      child: Column(
        children: [
          BlocBuilder<CounterBloc, int>( //BlocBuilder를 사용해 BLoC의 상태를 감시, 상태가 바뀌면 UI를 새로 그림
            builder: (context, count) {
              return Text('Count: $count');
            },
          ),
          IconButton(
            icon: Icon(Icons.remove),
            onPressed: () => context.read<CounterBloc>().add(Decrease()), //Increase 이벤트 전송
          ),
          IconButton(
            icon: Icon(Icons.add),
            onPressed: () => context.read<CounterBloc>().add(Increase()), //Decrease 이벤트 전송
          ),
        ],
      ),
    );
  }
}

```

**코드 실행 흐름**

1. 앱이 실행되면 `CounterBloc`이 만들어지고 초기 상태인 0으로 설정된다. 
2. 화면에는 “-”버튼, “Count: 0”, “+”버튼이 표시된다.
3. 사용자가 “+”버튼을 누르면
    1. `Increase`이벤트가 `CounterBloc`으로 전달된다.
    2. `mapEventToState`함수가 호출되어 숫자가 1 증가한다. 
    3. 새로운 상태(1)가 화면에 전달되고 “Count: 1”로 화면이 업데이트된다.
4. “-”버튼을 누르면 비슷한 과정으로 숫자가 감소한다. 

정리: *이벤트 → 상태 변경 → UI업데이트* 라는 명확한 흐름이 있어서 상태 관리가 깔끔하다. 

쉽게 말하자면 `CounterBloc`은 조작하는 사람, `BlocProvider`는 나눠주는 사람, `BlocBuilder`는 모니터, `context.read().add()`는 동작 이라고 할 수 있다. 

*test-driven development(테스트 주도 개발): 반복 테스트를 이용한 소프트웨어 방법론

작은 단위의 테스트 케이스를 작성하고 이를 통과하는 코드를 추가하는 단계를 반복해 구현한다.
