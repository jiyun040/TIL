# SingleChildScrollView
`Column` 또는 `Row`로 위젯들을 배치하거나 크기가 큰 위젯들을 배치하게 되면 화면의 크기보다 위젯이 더 큰 경우 발생하게 되는 `overflow` 에러가 난다. 

이런 경우 `SingleChildScrollView`로 에러를 해결할 수 있다. 

말 그대로 **스크롤을 가능하게** 하는 것인데 **자식을 하나만** 갖는다. 그 하나인 자식의 크기가 너무 커서 화면에 다 안나오고 삐져나간다던가 그럴 때 스크롤을 해 자식을 다 볼 수 있게 해주는 것이다. 

보통 `Row`나 `Column`의 위젯들과 자주 쓰이며 `SingleCHildScrollView`로 감싸면 스크롤이 가능해진다. 

단, 이것은 화면 전체를 스크롤 해주는 것이기에 어떤 특정 부분만 스크롤 하게 하려면 `Expended`를 사용해주면 된다. 

예를 들어 
```dart
Column(
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          const HeaderContainer(),
          const Padding(
            padding: EdgeInsets.all(20),
            child: SearchField(),
          ),
          Padding(
            padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 5),
            child: Text(
              '게시글 목록',
              style: PlymaTextStyle().list(color: PlymaColors.gray500),
            ),
          ),
          const Expanded(
            child: PostListview(),
          ),
        ],
      ),
 ```
 이렇게 `Expended`로 묶어주면 저 리스트 부분만 스크롤이 되지만
 
 `Expended`를 지우고 제일 위에 `SingleChildScrollView`로 `Column`을 묶어주면 헤더컨테이너부터 리스트 까지 전부 스크롤이 된다. 
 
 어느것을 사용할지는 그때그때 맞춰서 원하는걸 사용하면 될 것 같다. 
 
 리스트는 어디서나 많이 써먹기 때문에 관련된 것들을 많이 알아두고 여기저기 적용 해보면 좋을 것 같다. 
