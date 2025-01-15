### async/await

이것도 Future와 같이 비동기 작업을 할 때 사용하는 것 이다.

아래와 같은 코드에서 예를 들어보자

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
	print("get data!")
}

void fetchData(){
	print("finally!")
}
```

- 그냥 실행을 했을 경우

**start! get data! finally!** 의 순서로 출력이 된다. 

위에서부터 아래로 순차적으로 실행되고 있다. 

1. 먼저 두번째 함수에서는 임의의 String변수인 account를 만들고 이 값을 반환하도록 한다.
2. 그 다음 세번째 함수를 실행시킬 때 두번째 함수의 결과값으로 받은 account를 인자로 갖도록 한다.
3. 마지막 세번째 함수 내부 에서는 이 인자값을 받아 사용하게 코드를 수정한다.

1 > accessData()

```dart
String? accessData(){
	String? account;
	Duration time = Duration(seconds: 3);
	Future.delayed(time, (){
		account = "200만원";
		print("get $account data!");
	});
	return account;
}
```

이 코드를 보면 account 변수를 반환하는 것은 account에 200만원을 할당할 후 일어나야하는데 함수 내부에서 `Future.delayed`를 만나면서 `account = “200만원”` 보다 `return account`가 먼저 실행된다. 

2 > startStudying()

```dart
void startStudying(){
	getStart();
	String? account2 = accessData();
	fetchData(account2);
}
```

account 변수로 accessData의 값을 받아 fetchData의 인자로 넘겨준다.

3 > fetchData()

```dart
void fetchData(String? account){
	print("finally fetch $account !");
}
```

fetchData()의 인자로 받아서 출력을 해준다. 

→ **start! finally! null** (•••3초 후•••) **get 200만원 data!** 라는 결과가 나오게 된다. 

null이 왜 나왔냐면 세번째 함수에서 찍은 account가 null로 반환된 것 이다. 

왜냐하면 `Future.delayed`를 만나면서 `account`의 값이 할당되기도 전에 반환해버렸고, 그 반환값을 그대로 변수에 담아서 인자로 전달했기 때문이다. 

그치만 200만원을 그대로 쓰고 싶다면 account에 값이 할당된 이후에 account를 반환하면 된다. 이런 상황일 때 쓸 수 있는 것이 async await 이다. 

1 > 

```dart
Future<String?> accessData() async{
	String? account;
	Duration time = Duration(seconds: 3);
	await Future.delayed(time, (){
		account = "200만원";
	});
	return account;
}
```

→ `Future.delayed`의 값이 나올 때 까지 기다린다. 

2 >

```dart
void startStudying() async{
	getStart();
	String? account2 = await accessData();
	fetchData(account2);
}
```

→ `accessData()`의 실행이 끝날 때 까지 기다린다. 

**start! get 200만원 data! finally! 200만원** 이라고 우리가 원하는것 처럼 출력이 된다. 

Future, async/await 을 공부하긴 했는데 아직 헷갈리고 실제로 써보진 않아서 앞으로 더 공부하고 써봐야 할 것 같다.
