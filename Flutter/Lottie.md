> JSON 기반 애니메이션 파일
> 

**장점**

- 작은 파일 크기
    - GIF나 MP4와 같은 다른 형식과 품질은 같지만 크기가 훨씬 작다.
- 자유로운 크기 조정
    - 벡터에 기반하기 때문에 해상도의 영향을 받지 않는다.
- 다양한 플랫폼 지원, 라이브러리
    - iOS, 안드로이드, 웹, React Native에서 수정하지 않고 바로 사용할 수 있다.
- 상호작용
    - 애니메이션 요소가 드러나 있어 상호작용하도록 수정할 수 있고 스크롤링, 클릭, 호버링 같은 상호작용에 반응할 수 있다.

Lottie 애니메이션은 웹사이트에서 무료 애니메이션을 찾아 다운로드하거나 직접 제작할 수 있다.

### 패키지 설치

Flutter에서는 이런 Lottie JSON을 컨트롤할 수 있는 패키지가 있다. 

https://pub.dev/packages/lottie

`pubsec.yaml`파일에 다음과 같이 추가해 설치한다. 

```yaml
dependencies:
  flutter:
    sdk: flutter
  lottie: ^latest_version # 최신 버전으로 교체
```

### 애니메이션 표시

Lottie 애니메이션을 표시하는 방법은 Network로 사용하거나, assets로 사용, zip으로 사용할 수가 있다. 

1. Network

```dart
Lottie.network(
  'https://assets3.lottiefiles.com/packages/lf20_syycbook.json',
);
```

2. assets ( 물론 파일에 assets가 있어야 한다. 예: `assets/LottieLogo.JSON` )

```dart
Lottie.asset('assets/LottieLogo.json');
```

3. zip

```dart
Lottie.asset('assets/LottieLogo.zip');
```

### 제어

동적으로 제어하기 위해선 `AnimationController`를 사용해야 한다. 

- `TickerProviderStateMixin` 설정

  주로 `Stateful`에서 사용

```dart
class _LottieTestPageState extends State<LottieTestPage> with TickerProviderStateMixin {
  // ... 코드 ...
}
```

- 컨트롤러 생성, 해제

  `initState`에서 `AnimationController`를 생성하고, 위젯이 dispose될 때 `dispose` 메서드에서 컨트롤러를 해제하여 메모리 누수를 방지해야 한다.

```dart
class _LottieTestPageState extends State<LottieTestPage> with TickerProviderStateMixin {
  late final AnimationController _lottieController;

  @override
  void initState() {
    super.initState();
    _lottieController = AnimationController(vsync: this); // vsync에 this를 전달
  }

  @override
  void dispose() {
    _lottieController.dispose(); // 컨트롤러 해제
    super.dispose();
  }

  // ... 코드 ...
}
```

*vsync ( Vertical Synchronization (수직 동기화) 의 약자 ): 애니메이션 컨트롤러가 화면의 새로 고침 신호에 동기화되도록 해주는 역할

- 컨트롤러 주입, 재생 시간 설정

  생성한 컨트롤러를 `Lottie` 위젯의 `controller` 속성에 주입하고, `onLoaded` 콜백을 사용하여 Lottie 애니메이션의 실제 재생 시간을 컨트롤러에 설정한다.

```dart
Lottie.network(
  'https://assets3.lottiefiles.com/packages/lf20_syycbook.json',
  controller: _lottieController,
  onLoaded: (composition) {
    _lottieController.duration = composition.duration; // Lottie의 재생 시간을 컨트롤러에 설정
  },
);
```

- 애니메이션 재생, 반복
    - `forward()`: 처음부터 끝까지 한 번 재생
    - `repeat()`: 반복 재생
        - `reverse true`: 반복할 때 역방향으로 재생
        - `min`, `max`, `period`: 특정 구간만 반복하거나, 반복 재생 속도 조절

```dart
// 특정 구간 (0.3 ~ 0.8) 반복 재생 및 속도 조절 예시
onLoaded: (composition) {
  _lottieController.duration = composition.duration;
  _lottieController.repeat(
    min: 0.3,
    max: 0.8,
    period: _lottieController.duration! * (stop - start),
  );
}
```

- 애니메이션 정지, 재개
    - `stop()`: 정지
    - `forward(from: _lottieController.value)`: 정지했던 시점부터 애니메이션을 재개한다.`_lottieController.value`는 현재 컨트롤러의 진행 값(0.0 ~ 1.0)을 나타냅낸다.

### 사이즈

`width`, `height`, `fit`속성 이용

```dart
Lottie.network(
  'https://assets3.lottiefiles.com/packages/lf20_syycbook.json',
  width: 30,
  height: 100,
  fit: BoxFit.fill, // fill, contain, cover 등
);
```

*fill: 비율 무시하고 집어넣음 / contain: 비율 유지하면서 넣음, 여백이 생길 수 있음 / cover: 비율 유지하면서 꽉 들어갈 수 있게, 원본 사진 잘릴 수 있음

### 커스텀 로딩 화면

로딩 중 인디케이터를 사용하고 싶다면 `FutureBuilder`를 사용할 수 있다. 

```dart
class _LottieTestPageState extends State<LottieTestPage> with TickerProviderStateMixin {
  late final Future<LottieComposition> _composition;

  @override
  void initState() {
    super.initState();
    _composition = NetworkLottie(
            'https://assets3.lottiefiles.com/packages/lf20_syycbook.json')
        .load();
  }

  @override
  Widget build(BuildContext context) {
    return FutureBuilder<LottieComposition>(
      future: _composition,
      builder: (context, snapshot) {
        var composition = snapshot.data;
        if (composition != null) {
          return Lottie(composition: composition);
        } else {
          return const Center(child: CircularProgressIndicator()); // 로딩 중 표시
        }
      },
    );
  }
}
```
