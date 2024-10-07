# AppBar을 커스텀 클래스로 만들기
AppBar을 커스텀 클래스로 만들기 위해서는 `PreferredSizeWidget`을 사용해 주면 된다. 
`PreferredSizeWidget`은 **화면의 크기 및 위치와 관련된 정보를 제공하는 위젯**이다. 

주로 `AppBar`와 같은 상단 앱 바, `SliverAppBar`와 같은 스크롤 가능한 상단 앱 바 및 기타 커스텀 위젯에서 사용된다. 해당 위젯의 크기를 정확하게 제어하고 다른 위젯과의 상호작용을 관리할 수 있다. 

구현할 때, `PreferredSize`클래스를 사용해 위젯의 높이를 지정하고, **AppBar**의 높이는 **kToolbarHeight** 상수를 사용해 설정한다.

아래와 같이 써주면 된다. 
```dart
 class Appbar extends StatelessWidget implements PreferredSizeWidget {
  const Appbar({super.key});
  ...
  @override
  Size get preferredSize => const Size.fromHeight(kToolbarHeight);
 ```
`PreferredSizeWidget`을 사용하지 않았을 때는 왜 AppBar가 적용되지 않는지 답답했는데 찾아보니 저걸 꼭 써줘야만 커스텀 앱바 클래스를 적용시킬 수 있다. 
