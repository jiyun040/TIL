다양한 스타일이 혼합된 텍스트를 쓰고 싶을 때 주로 사용한다.

예를 들어 아래처럼 중간의 텍스트의 두께를 바꾸고 싶을 때 사용한다. 

```dart
RichText(
	text: textSpan(
		text: 'Hello ',
		style: DefaultTextStyle.of(context).style,
		children: const <TextSpan>[
			TextSpan(text: 'bold', style: TextStyle(fontWeight: FontWeight.bold),
			TextSpan(text: 'world!'),
		],
	),
)
```

```dart
RichText(
	text: TextSpan(
		children: [
			TextSpan(text: 'Hello ',
			TextSpan(text: 'bold', style: TextStyle(fontWeight: FontWeight.bold)),
			TextSpan(text: 'world!'),
		]
	)
)
```

위 코드 두개는 똑같지만 children으로 bold랑 world를 두기 싫다면 두번째 코드와 같이 할 수 있다. 

RichText는 TextSpan으로 구성된 트리로 이어진다. 

제일 상위의 RichText밑에 TextSpan(parent)가 있고, 그 밑에 또 TextSpan(children)들이 여러개로 있는 트리 구조이다. 
