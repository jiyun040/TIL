사용자가 목록을 끌어단여거 새로 고침을 수행할 수 있는 위젯이다. 

주로 스크롤 가능한 목록에서 사용되며, 사용자에게 데이터를 새로 고치는 기능을 제공한다.

**주요 속성**

- child: RefreshIndicator가 감싸는 스크롤 가능한 위젯이다. 일반적으로 ListView와 같은 위젯이 사용된다.
- onRefresh: 새로 고침 동작이 발생할 때 호출되는 콜백 함수이다. 
                    이 함수는 Future<void>를 반환하며 비동기 작업을 수행할 수 있다.
- color: Indicator의 색상을 지정한다.
- displacement: Indicator가 표시될 때 위쪽에서 떨어진 거리이다.
- strokeWidth: Indicator의 선 두께이다.

 

```dart
class RefreshIndicatorExample extends StatefulWidget {
  @override
  _RefreshIndicatorExampleState createState() => _RefreshIndicatorExampleState();
}

class _RefreshIndicatorExampleState extends State<RefreshIndicatorExample> {
  // _items 리스트에는 20개의 아이템이 포함됨
  final List<String> _items = List.generate(20, (index) => 'Item ${index + 1}');

  // 새로고침 함수
  Future<void> _refresh() async {
    await Future.delayed(Duration(seconds: 2)); // 2초 동안 대기 후

    setState(() {
      _items.shuffle(); // _items 리스트의 순서를 랜덤으로 섞음
    }); // UI 업데이트
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text('RefreshIndicator Example')),
      body: RefreshIndicator(
        onRefresh: _refresh, // 새로고침 시 _refresh() 실행
        child: ListView.builder(
          itemCount: _items.length, // 리스트 아이템 개수
          itemBuilder: (context, index) {
            return ListTile(
              title: Text(_items[index]), // 리스트 아이템 표시
            );
          },
        ),
      ),
    );
  }
}
```

**주요 포인트**

1. RefreshIndicator 사용:
- RefreshIndicator 위젯은 스크롤 가능한 위젯을 감싸서 사용한다.
- onRefresh 콜백은 데이터를 새로고침하는 로직을 포항해야 하며, Future<void>를 반환해야 한다.
2. 데이터 새로 고침:
- onRefresh 콜백 내에서 비동기 작업을 수행하여 데이터를 새로 고침 한다.
- 비동기 작업이 완료되면 setState를 호출하여 UI를 업데이트 한다. 
3. UI 업데이트
- setState를 사용해 데이터 변경 사항을 반영하고 UI를 그에 맞게 업데이트 한다.
