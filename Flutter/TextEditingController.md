# TextEditingController
- 편집 가능한 텍스트 필더의 컨트롤러
- 연결된 컨트롤러를 이용하여 텍스트 필드를 수정할 때마다 값을 업데이트할 수 있다.
- 편집이 가능한 TextField에 입력된 값을 가지고 오거나 입력된 값이 변경될 때 사용하는 클래스

사용하는 법은 
1. 선언을 해준다. 
2. override를 시켜준다. 
3. TextField에 선언된 TextEditingController를 연결시킨다. 
4. Textfmf 이용해 데이터를 가지고 온다. 

난 여기서 Text 대신 errorMessage 라는 것을 따로 선언해 사용했다. 

왜 이걸 찾았냐면 오류 메세지를 보내기 위해서였다. 

이걸 이용해 오류가 나타났을 때 어떤 메세지를 보낼지 설정 해주게 됐다. 

````dart
....
  final TextEditingController idController = TextEditingController();
  final TextEditingController passwordController = TextEditingController();
  String? errorMessage;

  void _login() {
    setState(() {
      if (idController.text.length != 4 || (passwordController.text.length <= 1 || passwordController.text.length > 8)){
          errorMessage = '학번 또는 비밀번호가 일치하지 않습니다.';
      } else {
        errorMessage = null;
      }
    });
  }
  ....
  ....
  ....
  if (errorMessage != null)
  	Align(
    	alignment: Alignment.centerLeft,
        child: Padding(
        	padding: const EdgeInsets.all(10),
            child: Text(
                errorMessage!,
                style: PlymaTextStyle().match(color: PlymaColors.azureBlue),
                      ),
                    ),
                  ),
````
이렇게 하면 조건을 만족하지 않았을 때 오류 메세지가 뜰 수 있게 해준다. 

`Align`과 `padding`을 이용해서 위치도 조정해줄 수 있다. 

에러메세지를 어떻게 보여줘야할지 진짜 힘들었는데 이렇게 쓰고 나니 그래도 하나라도 얻어간거 같아서 좀 좋은듯 하다. 

근데 이 방법이 틀릴 수도 있어서..
