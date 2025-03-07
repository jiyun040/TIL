# Controller
- UI 컴포넌트의 상태와 동작을 관리할 때 자주 사용되는 개념
- UI 요소와 기본 로직 혹은 데이터 사이의 중개자 역할
- 하나 혹은 여러 개의 위젯 상태를 관리하며 데이터를 보관하고 사용자의 입력을 처리

##### 유형
1. 텍스트 편집: 텍스트 필드의 콘텐츠 관리, 변경사항 추적, 필드에 표시되는 텍스트 제어
2. 에니메이션: 에니메이션 시퀀스 관리, 타이밍과 진행 제어
3. 스크롤: 스크롤 가능한 위젯의 위치를 모니터링하로 제어

##### 장점
1. 관심사 분리: UI 코드에서 비즈니스 로직을 분리시켜줌
2. 재사용 가능성: 재사용이 가능해 코드 중복을 줄일 수 있음
3. 텍스트 가능성 개선: 컨트롤러는 UI 컴포넌트와 독립적으로 단위 테스트 가능

코드에서는 `StatefulWidget`에서 사용이 된다. 

이걸 왜 사용하게 됐냐하면 로그인, 회원가입을 할 때 오류 메세지를 보내기 위해 사용했다. 

`controller`를 사용해 각각의 입력을 받을 때의 값을 저장해 잘못 입력되었는지 맞게 입력되었는지 알아내 오류메시지를 보내는 코드를 작성했다. 

난 `TextEditingController`를 사용했다. 
