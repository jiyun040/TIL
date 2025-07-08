플러터 전용으로 구축된 라이브러리

의존성을 주입해 상태의 변화를 효과적으로 관리하고 UI에 쉽게 반영할 수 있게 해준다. 

데이터의 흐름을 위젯 트리를 통해 쉽게 제어할 수 있도록 설계되었다. 

**특징**

- 플러터 전용으로 제작된 경량화된 시스템을 가지고, 다른 라이브러리들과도 호환성이 좋다.
- 다양한 상태 관리 방법을 지원해 유연성이 좋다.
- 위젯 트리 어디에서나 의존성을 제공한다.
- 비즈니스와 UI로직의 분리를 장려한다.

**작동 흐름**

1. 상태를 위한 Provider repository를 생성한다.
2. Provider 위젯을 사용해서 repository에 접근 가능하다.
3. 서브 위젯들이 Provider.Of에 접근해 상태를 소비(consume)한다. 
4. Repository는 변경 사항을 리스너들에게 알린다. 
5. Listenable provider들은 UI를 자동으로 리빌드한다. 

**장점**

- 간단한 위젯 기반 API를 사용 가능하다.
- 다양한 상태 관리 패턴과 잘 통합되어 초보자부터 전문가까지, 소규모부터 대규모 앱까지 널리 사용된다.
- 플러터 팀에서 권장하여 다양한 듀토리얼과 문서가 많다.

**단점**

- 이 개념이 낯선 개발자에게는 초기 설정과 사용 방법이 복잡할 수 있다.
- 상태 관리를 설정하고 사용하기 위한 보일러플레이트 코드가 반복되어 많이 필요할 수 있다.

```dart
//카운터의 상태를 관리하며 상태 변화가 있을 때 이를 알리는 역할
//ChangeNotifier를 믹스인(with)해 상태 변화를 감지하고 구독자에게 알림
class CounterModel with ChangeNotifier {
  int _count = 0; //내부에서 관리하는 private 변수, 외부에서 직접 접근하지 못하도록 int get count를 통해 읽기 전용으로 제공
  int get count => _count; //_count의 값을 읽을 수 있도록 하는 getter

  void increase() {
    _count++; //_count 값 증가
    notifyListeners(); //상태 변화 알림 -> 위젯이 다시 빌드
  }

  void decrease() {
    _count--; //_count 값 감소
    notifyListeners(); //상태 변화 알림
  }
}

//UI를 정의하고 ChangeNotifierProvider를 통해 CounterModel을 하위 위젯에 제공
class SimpleCounter extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<CounterModel>( //CounterModel인스턴스 생성하고 위젯에서 사용할 수 있도록 제공
      create: (context) => CounterModel(),
      child: Column(
        children: [
          Consumer<CounterModel>( //Cbuilder함수에서의 model로 CounterModel의 인스턴스를 받음
            builder: (context, model, child) => IconButton(
              icon: Icon(Icons.remove),
              onPressed: model.decrease, //감소 함수 호출
            ),
          ),
          Consumer<CounterModel>(
            builder: (context, model, child) => Text('Count: ${model.count}'), //UI를 다시 빌드
          ),
          Consumer<CounterModel>(
            builder: (context, model, child) => IconButton(
              icon: Icon(Icons.add),
              onPressed: model.increase, //증가 함수 호출
            ),
          ),
        ],
      ),
    );
  }
}
```

**코드 실행 흐름**

1. `CounterModel` 생성
    1. `ChangeNotifierProvider`가 `CounterModel`인스턴스를 생성해 하위 위젯에 제공
2. 초기 UI 렌더링
    1. “-”버튼, “Count: 0”텍스트, “+”버튼이 표시
3. 사용자 상호작용
    1. “-” 버튼 클릭: `model.decrease`호출 → `_count`감소 → `notifyListeners`호출 → 관련 UI 업데이트
    2. “+” 버튼 클릭: `model.increase`호출 → `_count`증가 → `notifyListeners`호출 → 관련 UI 업데이트
4. 상태 변화 감지 및 업데이트
    1. `Cousumer`위젯이 `model`의 상태 변화를 감지하고 UI를 즉시 업데이트

*위젯 트리: 코드로 작성한 위젯들을 트리 형식으로 표현한 것
