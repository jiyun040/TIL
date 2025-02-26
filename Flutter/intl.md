메시지 번역, 복수형 및 성별, 날짜/숫자 형식 지정 및 구문 분석, 양방향 텍스트를 포함한 국제화 및 현지화 기능을 제공한다. 

이 라이브러리를 사용하려면 의존성을 추가해줘야 한다.

터미널에서 진행한다.

```sql
$ dart pub add intl
```

```sql
$ flutter pub add intl
```

이렇게 하면 패키지의 pubsec.yaml에 추가된다. 

사용하기 위해선 똑같이 import 해주면 된다. 

**초기화**

사용하지 않는 로케일 데이터는 미리 로드하지 않도록 해 앱의 크기와 성능을 최적화 하는 단계가 필요하다. 

만약 날짜 형식만 지정하면 되는 경우에는 메시지, 숫자 또는 필요하지 않은 기타 항목들을 로드하는 데 시간이나 공간이 필요하지 않다. 

**숫자 형식 지정**

숫자 형식을 지정하려면 NumberFormat 인스턴스를 만든다. 

```dart
var f = NumberFormat('###.0#', 'en_US'); // 숫자와 나라 설정
print(f.format(12.345)); // 형식에 맞게 포매팅
// 결과 12.35
```

**날짜 형식화**

DateTime 형식을 지정하려면 DateFormat 인스턴스를 만든다. 

아래의 코드 외에 지원되는 것들이 많은데 그건 [DateFormat](https://www.notion.so/intl-1a61444778e2808c93d9c86a2e9ae2ed?pvs=21) 을 참고하면 된다. 

```dart
DateFormat.yMMMMEEEEd().format(aDateTime);
// 'Tuesday, February 26, 2025'
DateFormat('EEEEE', 'en_US').format(aDateTime);
// 'Tuesday'
DateFormat('EEEEE', 'ko').dormat(sDateTime);
// '화요일'
```
