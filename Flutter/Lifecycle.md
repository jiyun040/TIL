### 생명주기(Life Cycle)란?

**Foreground/Background 상태에 있을 때, 시스템이 발생시키는 event에 의해 App의 상태가 전환되는 일련의 과정**을 말한다. 

쉽게 말하면 사용자가 앱을 실행하는 과정에서 컴포넌트가 생성되고 사라지고 종료되는 일련의 과정에서 갖게되는 상태(state)를 말한다. 

#### 앱 상태

- Not running: 앱이 종료되거나 실행을 하고 있지 않은 상태
- Foreground: 화면이 나타난 상태
- Foreground Inactive: 상호작용이 불가능하지만 화면은 보이는 상태
예) 시스템 메시지
- Foreground Active: 상호작용이 가능하고 앱이 실행되고 있는 상태
- Background Active: 백그라운드 상태에서 앱이 실핼되고 있는 상태
예) 음악 실행
- Suspend: 빠르게 재실행하기 위해 메모리를 최고한 잡아놓은 상태

### 위젯 생명주기(Widget Life Cycle)

#### 위젯의 특징

- 화면에 보이는 모든 것이 위젯
- 변경이 필요하면 기존의 위젯을 모두 없애고 새로운 위젯으로 대체

#### Stateless Widget Life Cycle

StatelessWIdget은 단순히 화면을 구성하는 위젯이므로 StatefulWIdget에 비해 단순한 생명주기를 가진다. 

- Constructor 생성 후 `build()` 함수를 호출해 UI를 구성
- Lifecycle 동안 **단 한번의 `build()`함수를 실행**
- 자체적으로 상태(state)를 가지고 있지 않고 부모 위젯으로부터 전달받은 값이 변경되면 다시 빌드해 새로운 위젯으로 대체한다. 즉, 위젯의 변경이 일어나면 **처음부터 `Countructor`와 `build`가 다시 실행**되는 방식이다.


#### Stateful Widget Life Cycle

StatelessWidget과 달리 상태(state)를 가지고 있어서 보다 복잡하다. 

- `inState()`는 StatefulWidget이 생성될 때 **한번만 호출**되는 함수로 **상태(state)를 초기화**할 때 사용한다.
- 상태를 관리하기 위해서는 build()를 여러 번 불러야 하는데 Stateless 방식으로는 상태를 관리할 수 없다. 
그래서 2개의 클래스 형태로 구성한다.

1. StatelessWidget에서는 Counstructor 호출 후 build()함수를 호출 해 UI를 구성한다. 
2. StatefulWidget에서는 Countructor에서 createState()를 호출하고 상태(state)를 만든다. 
3. 이후 initState()를 호출해 초기화 하고, didChangeDependencies()가 불리고 변경이 필요한 상태인 dirty 상태가 된다. 
4. build()함수를 호출하고 화면에 UI가 나타나 clean상태가 된다. 
5. 만약 state를 변경하면 setState()에서 build()를 다시 호출하고 didUpdateWidget()의 반환에 따라 build()의 실행여부를 결정한다. 
6. didUpdateWidget()함수를 사용해서 렌더링 여부를 결정할 수 있어 성능 최적화를 진행하는 경우에는 didUpdateWidget()함수를 @override 함수로 구현할 수 있다. 

#### 함수

- createState()
객체 생성
해당 객체는 해당 위젯에 대한 모든 변경 가능한 상태가 유지되는 곳
- initState()
StatefulWidget이 생성될 때 한번만 호출되는 함수로 상태 초기화
반드시 super.initState()를 호출
- didChangeDependencies()
StatefulWidget이 생성될 때 호출되는 함수로 initState() 다음에 바로 호출
부모 위젯이나 자신의 상태가 변경될 때는 호출되지 않지만 참조하는 위젯(InheritedWidget이나 Provider)이 변경되면 호출
- build()
항상 didChangeDependencies() 다음에 호출
UI를 구현해야 하는 부분으로 build안에 로직이 많으면 앱의 퍼포먼스가 낮아진다. 
반드시 존재해야 하며 @override 재정의 대상이고, 반드시 Widget을 반환해야 한다.
- didUpdateWidget
부모 위젯에 의해 리빌드가 필요한 경우 build() 직전에 호출
부모 위젯의 변경으로 인해 애니메이션을 다시 실행할 필요가 있는 경우 사용
부모 위젯 변경에 따라 상태값을 초기화해야 한다면 이 함수 안에서 setState()를 통해 상태값을 다시 초기화
- deactive
상태 객체가 트리에서 제거될 때 호출
- dispose
위젯 객체가 위젯 트리에서 영구적으로 제거될 때 호출
상태 객체도 함께 제거되므로 deactive가 먼저 호출되어 상태 객체가 제거되었음을 알리고 dispose가 호출되어 위젯 객체가 삭제되었음을 알리는데 사용

*재정의 대상: 부모 클래스에서 정의된 메서드나 속성이 자식 클래스에서 동일한 이름으로 다시 작성될 수 있다. 

#### 패턴

1. 기본
위에서 말했던 3~7의 순서와 같고 삭제한다면 deactive 를 거쳐 dispose가 된다. 

2. 파라미터가 바뀌었을 때
1. **위젯이 삭제**된다. 
2. 새로 생긴 위젯이 생겨 Constructor는 실행되지만 **createState는 실행되지 않는다.** 
    기존에 삭제된 위젯의 상태를 찾아 **재활용**한다. 
3. State가 Clean인 상태에서 **didUpdateWidget(**)이 실행되면 Constructor로 인해 바뀐 값들을 기반으로 리빌드 한다. 
4. 리빌드 하기 위해 **dirty 상태**로 바뀐다. 
5. 변경된 값들을 기반으로 **build()**를 다시 실행한다.
6. **Clean 상태**로 돌아간다. 

3. setState를 실행했을 때
1. clean 상태에서 바꾸고 싶은 변수를 바꿔 **setState를 호출**한다.
2. **dirty 상태**가 된다. 
3. 바뀐 값에 따라 **build를 실**행한다.
4. 다시 **clean 상태**로 바뀐다. 

#### 위젯 호출 순서

StatefulWidget 생성

: Constructor → initState → didChangeDependencies → build

setState가 호출

: build

InheritedWidget 또는 Provider의 값이 변경

: didChangeDependencies → build

부모 위젯으로부터 전달받는 값이 변경

: didUpdateWidget → build

상태 객체가 제거

: deactive → build

위젯 객체가 제거

: deactive → dispose

#### 앱의 생명주기

- **detached**: 응용 프로그램이 플러터 엔진에 호스팅이 되었지만, 모든 뷰에서는 분리된 경우
엔진은 시작이 되었지만 아직 어떤 뷰에도 연결이 되지 않았거나, 뷰가 Navigator Pop에 의해 파괴 되었을 때의 상태이다.
- **inactive**: 응용 프로그램이 비활성 상태이며 사용자의 어떤 입력도 수신하지 않는 경우
Android와 iOS 모두 전화를 할 때 이 상태이다. 
이 상태에서의 앱은 언제든 일시정지(Pause) 상태가 될 수 있다고 가정을 해야 한다.
- **paused**: 사용자가 홈버튼 등을 눌러서 앱이 백그라운드 상태로 내려간 경우
즉, 사용자가 어플리케이션의 UI를 확인할 수 없는 경우
- **resumed**: 어플리케이션이 포그라운드에 있는 경우
응용프로그램이 사용자에게 보여지고 사용할 수 있는 상태
