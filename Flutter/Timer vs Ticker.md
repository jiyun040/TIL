### Timer

`dart:async`패키지에 정의된 Timer 클래스는 일정 시간이 지난 후에 콜백을 실행한다. 

단일 실행 ( `Timer(Duration, callback)` ) 또는

반복 실행 ( `Timer.periodic(Duration, callback)` )으로 사용할 수 있다. 

반복 실행은 그 시간마다 실행하지만 정확한 Duration을 보장하지는 않는다. 

- 지연이 없더라도 약간의 오차가 발생
- 지연 발생했을 때 콜백이 호출되지 않음
- 지연 해소되면 다음 콜백이 빠르게 호출

Timer는 비동기 작업을 예약하는 데 사용될 수 있지만, frame rate에 맞춰 애니메이션을 데어하는 데에는 최적화되어 있지 않다. 

즉, Timer는 "몇 초 뒤" 또는 "몇 초마다" 어떤 작업을 실행하는 데는 유용하지만, **화면의 새로 고침 주기에 정확히 맞춰서** 애니메이션을 부드럽게 제어하는 용도로는 적합하지 않다. 

```dart
import 'dart:async';

void main() {
  // 단일 실행 Timer
  Timer(Duration(seconds: 2), () {
    print('2초 후에 실행됩니다.');
  });

  // 반복 실행 Timer
  Timer.periodic(Duration(seconds: 1), (timer) {
    print('매 초마다 실행됩니다.');
    if (timer.tick >= 5) {
      timer.cancel(); // 5번 실행 후 타이머 종료
    }
  });
}
```

### 예시 코드

```dart
void startCountdown() {
  _timer?.cancel(); // 기존 타이머 있다면 취소해 중복 실행 방지
  initialCountdown = countdown; // 카운트다운 초기값 설정
  _timer = Timer.periodic(const Duration(seconds: 1), (timer) { // 1초 단위로
    if (countdown.inSeconds > 0) {
      setState(() {
        countdown -= const Duration(seconds: 1);
      });
    } else {
      timer.cancel();
      // 답변 화면 상태로 변경
      ref.read(answerStateProvider.notifier).state = AnswerState.answering;
      // 선택 인덱스 초기화
      ref.read(selectedIndexProvider.notifier).state = null;
      setState(() {
        countdown = initialCountdown;
      });
    }
  });
}
```

### Ticker

애니메이션 프레임을 생성하기 위해 설계됐다. 

매 프레임 마다 콜백을 호출하고 Flutter의 애니메이션 시스템과 밀접하게 연관돼있다. 

위젯의 생명주기와 연결됐고, 위젯이 화면에서 제거될 때 자동으로 중지된다. 

AnimationController는 내부적으로 Ticker를 사용하여 애니메이션의 진행을 관리한다.사용하게 되면 애니메이션의 시작, 중지, 역방향 재생 등을 쉽게 제어할 수 있다.

```dart
import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';

void main() => runApp(MyApp());

class MyApp extends StatefulWidget {
  @override
  _MyAppState createState() => _MyAppState();
}

class _MyAppState extends State<MyApp> with TickerProviderStateMixin {
  late final Ticker _ticker; // 화면이 새로 그려질 때마다 특정 콜백 함수를 호출하도록
  int _tick = 0;

  @override
  void initState() {
    super.initState();

    _ticker = createTicker((Duration elapsed) { // Ticker가 실행된 이후 경과 시간
      setState(() {
        _tick++;
      });
    })
    ..start();
  }

  @override
  void dispose() {
    _ticker.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: Scaffold(
        appBar: AppBar(title: Text('Ticker 예시')),
        body: Center(child: Text('Tick: $_tick')),
      ),
    );
  }
}
```

`Timer`는 일정 시간 후에 작업하는 데에 초점을 맞추고 있고, Flutter내에서 특별한 애니메이션 없이 시간 기반으로 일반적인 작업을 할 때 사용되고 Dart의 전체에서 사용할 수 있다. 

`Ticker`는 애니메이션과 같이 시간에 따라 연속적으로 변화는 작업에 초점이 맞춰져있고, 애니메이션과 연관돼있어 위젯의 수명 주기와도 연동되고 애니메이션을 쉽게 제어할 수 있다.
