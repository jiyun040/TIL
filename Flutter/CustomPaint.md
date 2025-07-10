> Canvas객체를 통해 직접 그리기를 제어할 수 있어 고급 그래픽 처리가 가능하다.
> 

### CustomPaint

원, 사각형, 경로 등 기본적인 도형부터 시작해 복잡한 그래픽도 자유롭게 그릴 수 있다. 

애니메이션의 정확한 타이밍, 속도 조절 등을 세세하게 설정할 수 있다. 

사용자의 입력에 반응해 동적으로 그래픽을 변경할 수 있다.

```dart
// 진행률(progress)에 따라 원형으로 채워지는 아크(arc)를 그리는 CustomPainter
class CircularProgressPainter extends CustomPainter {
  final double progress;
  final double strokeWidth;
  final Gradient gradient;

  CircularProgressPainter({
    required this.progress,
    required this.strokeWidth,
    required this.gradient,
  });

  @override
  void paint(Canvas canvas, Size size) {
	  // 원의 중심 좌표 (가운데 정렬)
    final center = Offset(size.width / 2, size.height / 2);
    // 반지름
    final radius = (size.width - strokeWidth) / 2;
    // 그라디언트를 만들기 위한 기준 사각형
    final rect = Rect.fromCircle(center: center, radius: radius);

    final paint = Paint()
      ..shader = gradient.createShader(rect) // 색상을 그라디언트로 설정
      ..strokeWidth = strokeWidth // 선 두께
      ..style = PaintingStyle.stroke // 선만 그리고 내부 비움
      ..strokeCap = StrokeCap.round; // 둥글게

    const startAngle = -pi / 2;
    final sweepAngle = -2 * pi * progress;

		// 실제 화면에 그림
    canvas.drawArc(
      rect,
      startAngle,
      sweepAngle,
      false, // true면 중심까지 채우기, false면 선만 채우기
      paint,
    );
  }

	// 상태가 바뀔 때 마다 다시 그려지는걸 방지
  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) {
	  // 이전의 painter가 같은 CircularProgressPainter인지 확인, progress 값이 다를 경우만 다시 그림
    return oldDelegate is CircularProgressPainter && oldDelegate.progress != progress;
  }
}
```

### 일반 애니메이션

Flutter의 애니메이션 프레임워크를 사용해 위젯의 속성들을 시간에 따라 변화할 수 있다. 

이것들은 `AnimatedContainer`, `AnimatedOpacity`등의 위젯을 사용해 쉽게 구현이 가능하다. 

애니메이션을 적용할 속성을 정의하고 시간에 따라 어떻게 변화할지를 설정하면 된다.

Flutter 엔진이 애니메이션의 렌더링을 최적해, 복잡한 계산 없이도 부드러운 애니메이션의 효과를 구현할 수 있다.
