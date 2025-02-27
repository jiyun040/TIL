우리가 흔히 생각하는 달력이다. 

### 1. 기본 캘린더

table calendar를 사용하기 위해서는 먼저 패키지를 하나 설치해야 한다. 

yaml 파일에 들어가면 dependencies라는 부분이 있는데 그곳에

```dart
table_calendar: (버전)
```

패키지를 쓰고 Pub get을 한다. 

잘 설치가 되었다면 작성하던 파일로 돌아가서

```dart
import 'package:table_calendar/table_calendar.dart';
```

이렇게 적어주면 사용할 수 있다. 

Table Calendar의 기본 요소에는 3가지가 있다. 

- focusedDay: 달력에서 기준이 되는 날짜
- firstDay: 달력의 시작 날짜
- lastDay: 달력의 마지막 날짜

### 2. 한글 설정

한글로 설정해주지 않으면 영어로 되어있기 때문에 한국어로 설정하기 위해서는 패키지를 하나 더 설치해야 한다. 

```dart
intl: (버전)
```

위와 같은 방법으로 설치 한 후 똑같이 import 하면 된다.

```dart
import 'package:intl/date_symbol_date_local.dart';
```

main.dart 파일에서

```dart
void main() async{
	await initializeDateFormattimg();
	runApp(materialApp(home: MyApp()));
}
```

메인 함수 부분에 코드를 추가하고 locale도 한국어로 바꿔준다. 

```dart
locale: 'ko_KR';
```

### 3. 헤더 변경

기본적으로 캘린더를 한 달, 두 주, 한 주로 볼 수 있게 상태를 변경할 수 있는 버튼이 오른쪽 상단에 만들어진다. 

이 버튼을 사용하지 않는다면

formatButtonVisible: false로 해주면 되고, titleCenterd: true로 하여 상단의 날짜를 중앙으로 옮길 수 있다.
