자식 위젯들을 겹치게 배치할 수 있는 위젯

자식은 Positioned / Non-positoned로 구분할 수 있다. 

Positioned 위젯으로 자식 위젯을 래핑하면 Positioned 상태가 되고, Non-positioned는 Positioned 위젯을 사용하지 않은 일반 위젯으로 Stack의 Alignment에 따라서 자식 위치가 결정된다. 

기본값은 왼쪽에서 오른쪽 환경의 경우 왼쪽 상단 모서리, 오른쪽에서 왼쪽 환경의 경우 오른쪽 상단 모서리 이다. 

#### 1. Non-positioned

```dart
@override
Widget build(BuildContext context) {
	return Scaffold(
		body: Stack(
			chidren: [
				Container(
					width: 100,
					height: 100,
					color: Colors.cyan,
				),
			],
		),
	);
}
```

Stack의 자식 위젯으로 Container 한 개를 사용하고 Positioned 위젯으로 래핑하지 않았고 Alignment 값을 설정하지 않았기에 왼쪽 상단에 가로세로가 100인 Container가 위치한다. 


#### 2. 배치 순서

Stack은 첫 번째 자식이 맨 아래에 위치한다. 

그래서 여러 위젯을 겹쳐서 사용하기 위해서는 자식 위젯들의 순서를 잘 정해야 한다. 

```dart
@override
Widget build(BuildCOntext context) {
	return Scaffold(
		body: Stack(
			children: [
				Container(
					width: 200,
					height: 200,
					color: Colors.cyan,
				),
				Container(
					width: 150,
					height: 150,
					color: Colors.pink,
				),
			],
		),
	);
}
```

자식 위젯으로 가로세로가 200인 Container가 먼저 왔고, 그 다음으로 가로세로가 150인 Container가 왔기 때문에 렌더링 될 때 150의 Container가 더 위에 그려진다. 

Stack alignment를 설정해주면 자식 위젯들의 위치를 일괄적으로 변경할 수 있다. 

alignment의 값을 center로 입력하면 Non-positioned인 자식 위젯들이 Stack의 가운데로 이동한다. 

```dart
@override
Widget build(BuildContext context) {
	return Scaffold(
		body: Stack(
			alignment: Alignment.center,
			children: [
				Container(
					width: 200,
					height: 200,
					color: Colors.cyan,
				),
				Container(
					width: 150,
					height: 150,
					color: Colors.pink,
				),
				Container(
					width: 100,
					height: 100,
					color: Colors.orange,
				),
			],
		),
	);
}
```

#### 3. Positioned

Stack의 자식으로서 위치를 결정하는 데 사용한다. 

top, left, right, bottom 이렇게 4개의 특성이 있고, 4개의 값을 모두 입력하지 않거나 null을 입력한다면 Stack의 사이즈는 Positioned의 자식 위젯의 사이즈로 강제된다. (→ 자식 위젯의 크기에 맞춰서 크기가 결정된다는 뜻)

```dart
@override
Widget build(BuildContext context) {
	return Scaffold(
		body: positioned(),
	);
}

Widget positioned() {
	return Stack(
		childrec: [
			 Positioned(
				 child: Container(
					 width: 200,
					 height: 200,
					 color: Colors,blue,
					),
				),
				Positioned(
					top: 150,
					left: 150,
					child: Container(
						width: 100,
						height: 100,
						color: Colors.orange,
					),
				)
			],
		);
	}
	
```

2개의 자식 위젯이 있고 하나는 Positioned를 사용했지만 아무 값을 입력하지 않았고, 하나는 top과 left를 입력했습니다. 

아무 속성도 입력하지 않은 Positioned 위젯의 자식 사이즈로 Stack이 강제 되었다. 그래서 top과 left를 150으로 입력하고, 가로세로를 100으로 입력한 orange의 Container는 조금만 보인다. 

이런 특성 때문에 Positioned 위젯에서는 상하 중 한 개, 좌우 중 한 개의 값을 입력하는 것이 필요하다. 

왜냐하면 적어도 두 개의 값이 있다면 그것을 기준으로 자식 위젯의 사이즈를 계산해 화면에 그려줄 수 있기 때문이다. 

```dart
@override
Widget build(BuildContext context) {
	return Scaffold(
		body: positioned(),
	);
}

Widget positioned() {
	return Stack(
		children: [
			Positioned(
				top: 30,
				left: 30,
				child: Container(
					width: 170,
					height: 170,
					colod: Colors.purple
				),
			),
			Positioned(
				top: 70,
				left: 70,
				child: Container(
					width: 160,
					height: 160,
					color: Colors.yellow
				),
			),
			Positioned(
				top: 120,
				left: 120,
				child: Container(
					width: 120,
					height: 120,
					colod: Coolors.orange
				),
			),
			Positioned(
				top: 170,
				left: 170,
				child: Container(
					width: 80,
					height: 80,
					color: Colors.red
				),
			),
		],
	);
}
```

#### 4. Positioned.fill

Positioned 자식 위젯의 사이즈를 입력하고 left, right를 0으로 입력하면 자식 위젯의 가로 사이즈는 무시되고, Stack의 사이즈 만큼 확장된다. (→ 자식 위젯이 부모 Stack의 왼쪽과 오른쪽의 끝에 딱 맞춰지기에)

```dart
Widget positionedFill() {
	return Container(
		width: 400,
		height: 400,
		color: Colors.grey,
		child: Stack(
			children: [
				Positioned(
					left: 0,
					right: 0,
					child: Container(
						width: 200,
						height: 200,
						color: Colors.blue,
					),
				),
			],
		),
	);
}
```

가로세로 200의 Container를 Positioned로 래핑하고 좌우를 모두 0으로 입력했더니 Stack의 상위Container의 가로 사이즈인 400으로 가로 사이즈가 강제되었다. 

```dart
Widget positionedFill() {
	return Container(
		width: 400,
		height: 400,
		color: Colors.grey,
		child: Stack(
			children: [
				Positioned.fill(
					child: Container(
						width: 200,
						height: 200,
						color: Colors.blue,
					),
				),
			],
		),
	);
}
```

만약 상하좌우를 모두 0으로 입력하면 자식 위젯의 사이즈는 모두 무시되고, 확장 가능한 크기로 확장된다. 

그리고 상하좌우를 모두 0으로 입력하는 것이 기본값으로 세팅된 위젯이 Positioned.fill 위젯이다.

---
부모 위젯의 크기를 기준으로 자식 위젯들을 배치할 수 있어서 복잡한 레이아웃을 만들 때 도움이 되고, 

정확한 위치에 배치 할 수도 있어서 UI를 직관적으로 짤 수 있지만, 

겹치지 않게 사용하기 위해서는 어떤 것이 자식으로 가야하는지 더 생각해 봐야 하는 어려움도 있었다. 

또, left와 right를 사용했을 때 예상치 못 한 확장이 일어날 수도 있다. 

그렇지만 레이아웃을 자유롭게 사용할 수 있는 것 이라고 생각되었다. 
