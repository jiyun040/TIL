# SearchBar
`SearchBar`을 사용하면 그냥 기본 검색창이 보인다. 

leading 위젯을 추가할 때는 보통 `Icon`이나 `IconButton`, 기타 이미지가 들어가는 경우가 많다. 

아래와 같이 해주게 되면
````Dart
SearchBar (
	leading: Icon(Icons.search),
   )
````
왼쪽에 돋보기 아이콘이 생긴 검색창이 만들어진다. 

`trailing` 위젯을 추가할 때는 list로 만들어준다. 

다른것들과 같이 `backgroundColor`나 `shadowColor`, `elevation`도 설정해줄 수 있다. 

여기서 처음 본 것은 `overlayColor`, `constraints` 라는 것이다. 

이건 선택했을 떄, 나타나는 활성화 색상을 설정해주는 것이다. 

````dart
SearchBar (
	trailing: [Icon(Icons.search)],
    overlayColor: MaterialStatePropertyAll(Colors.red),
   ),
````
위와 같이 해주면 선택했을 때의 색이 빨간색으로 바뀌게 되는 것을 볼 수 있고, `trailing`으로 설정했기에 오른쪽에 돋보기 모양의 아이콘이 생기게 된다. 

`constraints`는 검색창의 크기를 설정해주는 것이다.

`BoxConstraints`를 이용해 최대/최서 크기를 설정할 수 있다. 
````dart
SearchBar (
	trailing: [Icon(Icons.search)],
    contraints: BoxContraints(maxWidth: 300, maxHeight: 100),
   ),
````
또, `shape`로 검색창의 모양을 바꿀 수 있고 `side`를 이용해 아웃라인을 설정해줄 수 있다. 

이 외에도 많은 파라미터들이 있다.
***
티스토리 - '뇌님의 관심사' 참고
