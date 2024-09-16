### 깃크라켄에서 깃플로우 사용해 feature 브랜치 만들고 PR 보내기
<hr>

#### *feature 브랜치 만들기*
1. develop인 상태로 준비해 놓는다. 
2. GITFLOW에 들어간 후 develop을 선택해 feature로 선택한다. 
3. feature의 이름은 `feature/기능이름`으로 설정한다. 
- 이때 이름은 띄어쓰기를 못 하니 `-`로 구분한다.
 - 예를 들어 `feature/로그인-뷰-추가`같이 한다. 
 - Current branch를 선택한다. 
 4. Start Feature을 누르면 끝.

#### *PR 보내기*
1. push를 보낸 상태에서 우클릭을 해 pull request를 선택한다.
2. To Repo를 main으로 선택한다. 
3. PR제목은 `PR :: [#이슈번호] 이슈제목`으로 설정한다. 
4. Reviewer를 설정해주고 보내면 끝.


여기서의 PR은 merge해주는 것과 같다고 생각하면 될 것 같다. 
