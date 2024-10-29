# floatingActionButton
- 화면 오른쪽 하단에 버튼이 생성된다. 

- `floatingActiongButton: FloatingActionButton()`으로 사용한다. 
- 원래는 원형의 모양이였는데 `material3`가 적용된 후 부터 둥그런 사각형 모양이 되어서 
```dart
MaterialApp(
	theme: ThemeData(useMaterial3: false);
````
로 해줘야 원형으로 나타나게 된다. 


### 모양 변경
`shape`라는 파라미터를 사용해 모양을 바꿔줄 수 있다.
1. 기본적으로 둥근 모서리의 사각형이 적용된다. 
2. 위와 같이 해줘 원형으로 바꿀 수 있다. 
3. `RoundedRectangleBorder()`로 설성해 사각형으로 만들 수 있다. 
4. 라벨 + 아이콘은 `FloatingActiongButton.extended`를 사용하고, `label`과 `Icon`을 써주면 된다. 
5. 가운데에 배치하려면 `floatingActionButtonLocation: FloatingActionButtonLocation.centerDicked`로 해준다. 
