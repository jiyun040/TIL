### TextField란?
사용자에게 글을 입력받는 위젯 → 출력할 문자열을 정렬하거나 스타일 지정 가능하다. 

#### TextEditing Controller 
텍스트 필드에 사용자가 입력한 글을 가져와 서버에 전송하거나 DB에 저장하는 등 다양하게 이용한다. 
이용 시 입력된 데이터 획득 등의 작업이 가능하다. 

#### InputDecoraton
텍스트 필드에 라벨이나 테두리 설정 등 다양하게 꾸미기 위해선 decoration 속성에 InputDecoration을 지정해줘야 한다. 
- labelText: 라벨 문자
- helperText: 아래에 출력되는 설명 문자
- hintText: 입력 상자 안에 출력되었다가 글 입력 히 사라지는 문자
- errorText: 아래에 출력되는 오류 문자
- prefixIcon: 입력 앞 부분에 고정으로 출력되는 아이콘 이미지
- counterText: 아래에 출력되는 문자
- border: 테두리 지정
	OutlineInputBorder, UnderlineInputBorder
errorText를 지정하면 텍스트 필드의 테두리는 자동으로 빨간색이 되며 helperText에 지정한 문자는 출력되지 않는다. 

#### keyboardType
기본적으로 문자를 입력하는 키보드지만 숫자만 입력하는 곳에서는 숫자만 나타나게 할 수 있다.
- TextInputType.number: 숫자
- TextInputType.text: 문자 (기본)
- TextInputType.phone: 전화번호
- TextInputType.emailAddress: 이메일 주소
- TextInputType.url: URL

키보드로 입력을 할 때 오버플로우가 나타나는 경우도 있는데 이때 해결방법은 크게 두 가지가 있다. 
1. resizeToAvoidBottomInset: 주로 키보드가 올라와도 텍스트 필드 영역이 보일 때 사용한다. 
   Scaffold의 속성인 resizeToAvoidBottomInset을 false로 설정한다. 
2. SingleChildScrollView: 주로 키보드가 올라왔을 때 필드 영역이 온전히 보이지 않고 스크롤할 때 사용한다. 
   Scaffold의 body 속성에 적용된 위젯을 감싸준다. 

#### obsecureText
보통 비밀번호 처럼 텍스트를 감춰야 할 때 사용하고, 속성을 true로 설정한다. 
