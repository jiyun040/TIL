플러터의 상태 관리 뿐만 아니라, 의존성 관리 및 라우트 관리도 할 수 있는 효율적이고 간단한 라이브러리 이다. 

반응형/비반응형 상태 관리를 모두 지원하며, 매우 낮은 오버헤드로 빠른 실행 속도를 제공한다. 

**특징**

- GetX는 매우 직관적인 API를 좋은 성능에 제공해 러닝 커브가 낮은 편이다.
- 라우팅, 의존성 관이 등 다양한 추가 기능을 내장하고 있다.
- 상태를 UI 코드로부터 쉽게 분리할 수 있고, 상태 변경 시 UI를 자동으로 새로고침 해준다.
- 적은 양의 보일러플레이트 코드를 요구한다.

**작동 흐름**

1. 상태를 위한 GetXController 클래스를 생성한다.
2. 컨드롤러는 observable한 프로퍼티들을 가지고 있다.
3. 컨트롤러의 상태를 업데이트 한다.
4. GetConsumer가 변경을 감지한다.
5. 상태 변경에 따라 UI를 자동으로 재구성한다. 

**장점**

- 매우 가볍고 빠르며 간단한 문법을 가져서 배우기 쉽다.
- 기존 비즈니스 로직 코드를 재사용 하기 원활하다.
- 세밀한 상태 관리와 라우팅 네비게이션이 가능하다.

**단점**

- 과도한 편의성: 너무 많은 기능이 하나의 라이브러리에 통합되어 있어 프로젝트의 복잡성이 증가할 수 있다고도 한다.

```dart
//카운터의 상태 관리하는 Controller 클래스
class SimpleCounterController extends GetxController { //BetXController를 상속받아 GetX 상태 관리 기능 활용
  var count = 0.obs; //Observable 변수로 상태를 감시, UI와 상태가 자동으로 동기화

  //카운트를 1증가, 1감소 시키는 함수
  void increase() => count++; 
  void decrease() => count--;
}

//UI를 정의하는 StatelessWidget, SimpleConuterController와 연동되어 버튼 클릭 시 카운터 값을 조작
class SimpleCounter extends StatelessWidget {
  final SimpleCounterController controller = Get.put(SimpleCounterController()); 
  //SimpleCounterController인스턴스를 메모리에 등록하고 앱의 다른 부분에서 재사용 가능하게 만듦
  //의존성 주입을 통해 컨트롤러를 간편하게 사용할 수 있도록 함

  @override
  Widget build(BuildContext context) {
    return Column(
      children: [
        IconButton(
          icon: Icon(Icons.remove),
          onPressed: controller.decrease, //감소 함수 연결
        ),
        Obx(() => Text('Count: ${controller.count}')), //상태 변화 감시 (핵심 기능, 값이 변경될 때마다 내부의 UI가 자동으로 다시 빌드)
        IconButton(
          icon: Icon(Icons.add),
          onPressed: controller.increase, //증가 함수 연결
        ),
      ],
    );
  }
}
```

**코드 실행 흐름**

1. 앱 초기화
    1. `SimpleCounterController` 가 메모리에 생성(`Get.put`)되고 `count` 가 0으로 초기화됨
2. UI
    1. 화면에 “-” 버튼, “count:0” 텍스트, “+” 버튼이 표시됨
3. 버튼 클릭
    1. “-” 버튼 클릭: `controller.decrease()`가 호출되어 `count`값이 감소
    2. “+” 버튼 클릭: `controller.increase()`가 호출되어 `count`값이 증가
4. 상태 변화 감지
    1. `Obx`위젯이 `controller.count`의 변화를 감지하고 UI를 즉시 업데이트 
---
*오버헤드(overhead): 어떤 처리를 하기 위해 들어가는 간접적인 처리 시간, 메모리 등

만약 A라는 처리를 단순하게 실행 한다면 10초가 걸리는데 안전성을 고려하고 부가적인 B라는 처리를 추가한 처리 시간이 15초가 걸렸다면 오버헤드는 5초가 된다. 

*보일러플레이트 코드: 최소한의 변경으로 여러 곳에 재사용되며, 반복적으로 비슷한 형태를 띄는 코드

*러닝 커브(Learning Curve): 특정 기술 또는 지식을 실제 필요한 업무와 같은 환경에서 효율적으로 사용하기 위해 드는 학습 비용 / 특정 기술을 습득할 때 처음에는 학습 효과가 더디다가 어느 정도 이해를 하고 나면 빠르게 습득하고 후에는 다시 더뎌지는 곡선

*의존성 주입(Dependency Injection): 외부에서 두 객체 간의 관계를 결정해주는 디자인 패턴, 인터페이스를 사이에 둬서 클래스 레벨에서는 의존관계가 고정되지 않도록 하고 런타임 시에 관계를 동적으로 주입해 유연성을 확보하고 결합도를 낮출 수 있게 해줌
