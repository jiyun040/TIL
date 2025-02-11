#### Instance

Class를 기반으로 생성되며 Class의 기능과 속성들을 동일하게 가지고 있는 프로그래밍 상에서 사용되는 대상

Class가 생성되고 생성된 Class가 사용되면서 메모리에 할당되었을 때 이를 Object(객체)라고 한다. 이 과정들을 인스턴스화 라고 한다.

Class가 Object로 선언되고 Object가 메모리에 할당되어 실제로 사용될 때가 Instance다.

#### Class

**같은 설계도로 만든 서로 다른 집**

Person 이라는 Class를 생성한다. 

```dart
class Person {
	final String name;
	final int age;
	
	const Person(this.name, this.age);
}

void main(){
	Person person1 = Person('jiyoon', 18);
	Person person2 = Person('seungyeon', 19);
	
	print(person1.name); // jiyoon
	print(person2.name); //seungyeon
}
```

Person 이라는 객체를 선언하는 것은 Constructor(생성자)를 통해 Person객체를 인스턴스 했다. 실제로 메모리 공간에 Person 객체의 인스턴스가 생긴 것이다. 

person1과 person2는 다른 메모리 공간을 사용해 독립적으로 존재하기에 각 객체가 다른 값을 가질 수 있다. 

#### Static

**공용 메모장에 적어두고 모두가 공유하는 정보**

정적이라는 뜻이고 즉, 고정되어있다는 의미이다. 

Static으로 선언되면 프로그래밍의 실행 부터 종료 까지 동안 메모리가 고정되었다는 것이다. 

일반적인 Class가 생성될 때 인스턴스화의 과정을 거치는 것과는 다르게 Static 구문은 프로그래밍이 실행되어 메모리 공간에 올려졌을 때 단 한번만 메모리의 공간에 할당된다. 

이렇게 생성된 Static 구문은 메모리를 공유하기 때문에 정보 또한 공유된다. 

불필요한 구문을 Static으로 생성하면 메모리 영역을 잡게 되어 퍼포먼스 면에서 좋지 않을 수 있이게 주의해서 사용해야 한다. 

```dart
class Person{
	static final String name = 'jiyoon';
}

void main(){
	print(Person.name); // jiyoon
	
	Person.name = 'seungyeon';
	
	print(Person.name); // seungyeon
}
```

위의 코드는 문자열 name 변수를 Static변수로 생성했다.

위와 같이 Static 구문으로 생성된 경우 별도의 인스턴스화 과정 없이 시작될 때 이미 할당되었기에 바로 사용이 가능하다. 

#### Singleton

**한 동네에 하나뿐인 사람**

단 하나의 인스턴스만 생성하는 디자인 패턴 기법이다. 

```dart
class Person {
	static final Person _instance = Person._internal(); // 유일한 인스턴스
	
	factory Person() {
		return _instance;
	}
	
	Person._internal(); // 내부 생선자
}

void main() {
	Person person1 = Person();
	Person person2 = Person();
}
```

Person을 여러 번 호출해도 항상 같은 인스턴스가 반한된다. 즉, 한번만 생성되고 계속 같은 객체를 공유하는 구조이다. 

Firebase, SharedPreferences 같은 라이브러리가 이 Singleton 패턴을 사용한다. 

Class는 객체를 인스턴스화 할 때 마다 Instance가 매번 생성된다. 

하지만 Singleton을 단 하나의 Instance만 생성하도록 강제한다. 

Static은 Class, Singleton과 다르게 인스턴스화의 과정을 거치지 않고 프로그램 실행 시 별도의 메모리 공간에 할당시키는 방법이다. 

| 개념 | 특징 | 예제 |
| --- | --- | --- |
| Class | 매번 새로운 객체(인스턴스)생성 | 같은 설계도로 여러 개의 집을 짓는 것 |
| Static | 한번만 메모리에 올라가고 모든 곳에서 공유 | 이름이 같은 공용 데이터 |
| Singleton | 객체가 단 하나만 존재 | 마을에 오직 한 명만 있는 사람 |
