### Future

현재에는 실제적으로 존재하지 않지만 미래에 무언가가 구체적인 결과물로 나타나서 실제적인 객체로 반환된다는 약속 이다.

아직 개봉하지 않은 박스라는 예를 들어보자

Future 클래스를 사용하면 Future라는 객체 즉, 박스 하나가 생성되고 후에 연락을 받고 열어보면 String, int••• 실제적인 객체가 박스 안에 존재한다 라고 할 수 있다. 

아래와 같이 원하는 데이터형을 지정해줄 수 있다.

`Future<String>.....`

주로 `Future.delay(duration, 함수())` 이런 식으로 delay메소드와 자주 쓴다.

비동기 처리 할 때 `delay`를 사용한다.

- `sleep` 함수도 늦춰지긴 하지만 어떤 일이 일어나게 되냐면

```dart
void main(){
	startStudying();
}

void startStudying(){
	getStart();
	accessData();
	fetchData();
}

void getStart(){
	print("start!");
}

void accessData(){
	print("start!")
}

void fetchData(){
	print("finally!")
}
```

위와 같은 코드를 기본으로 할 것이다.

```dart
void accessData(){
	Duration time = Duration(seconds: 3);
	sleep(time);
	print("get data!");
}
```

start! •••3초 뒤••• get data! finally! 라는 결과가 나오게 된다.

즉, main() 함수와 startStudying() 함수는 순차적으로 실행되었다.

---

- Future의 `delay`를 사용한다면

```dart
void accessData(){
	Duration time = Duration(seconds: 3);
	sleep(time);
	Future.delayed(time, (){
		print("get data!");
	});
}
```

start! finally! •••3초 뒤••• get data! 라는 결과가 나오게 된다.

즉, 두번째 실행 함수가 3초동안 연기되는 동안 세번째 함수가 먼저 실행된 것이다. 


`Future.delay()` 함수를 만나면 멈추고 그 다음 코드를 먼저 실행 하라고 한다.

→ 그래서 Future 클래스는 비동기적인 동작을 하고 싶을 때 사용할 수 있다는 것이다.
