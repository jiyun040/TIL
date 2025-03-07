# Github, GitKraken
### Git 과 Github의 차이점
**Git**

: 로컬에서 관리되는 버전 관리 시스템 (시간에 따라 파일의 변경사랑을 추적, 기록)

로컬 저장소를 사용 -> **다른 사람이 나의 작업 내용을 알 수 없음**

**Github**

: 개인의 로컬 서버 밖에서 Git 버전 프로젝트를 공유하고 기록하는 온라인 데이터 베이스

저장소를 깃허브에서 제공해 주는 클라우드 서버를 이용

클라우드 서비스이기에 **다른 사람들과 협업 시 소스코드 공유가 가능**



**Github 기본 용어**
1. Local / Remote

-Local: 사용하고 있는 컴퓨터

-Remote: 원격 저장소

2. Repository (repo, 저장소)

-프로젝트가 존재하는 공간

3. Branch (브랜치)

-Repository의 공간에서 독립적으로 어떤 작업을 하기 위한 공간

4. Commit

-소스코드의 업데이트를 확정, 확정된 순간의 코드 상태를 메시지와 함계 Git Repo에 저장

*로컬 저장소에는 변경이 반영되지만, 원격 저장소에는 반영되지 않은 상태 (Push를 해줘야 반영이 됨)

5. Pull / Push

-Pull: 원격저장소의 내용을 로컬저장소에 끌어오는 것

-Push: Commit한 내용을 원격저장소에 업로드


**Github 기본 프로세스**

- 수정 내역을 원격저장소에 내보내려면 *git add* -> *git commit* -> *git push*의 과정을 거쳐야 함
- 수정 내역을 받아올 때는 *git fetch*
- 내가 수정한 내역이 원격 저장소에 있는 내역과 다를 수 있기에 *git merge*를 수행해 자신의 컴퓨터에 있는 소스코드를 원격저장소와 맞춤


**GitKraken**

: Git 소스를 GUI로 편리하게 관리할 수 있는 툴

**사용이유**

: 브랜치들이 어떻게 뻗어나가는지 한눈에 보임

직관적으로 사용 가능


**깃허브와 깃크라켄 사용방법**

1. 깃허브에서 `New`를 눌러 레포의 이름과 퍼블릭으로 할건지 프라이빗으로 할건지 정해준 후 `리드미 파일`을 선택해 하나의 레포를 만들어준다. 
2. `코드`를 눌러 `HTTPS`를 선택해 URL을 복사한다. 
3. 깃크라켄에서 `Clone a Repo`를 선택한 후 파일을 지정해준 뒤 복사한 링크를 붙여넣어준다. 

위와 같이 해준다면 레포를 만들고 깃크라켄과의 연결도 한 것 이다. 

주의할 점이 몇가지가 있는데 

첫번쨰로 개발은 main에 하지 않으며 develop 브랜치를 만들고 feature 브랜치를 만들어 개발해 올린 후 develop으로 머지 한다. 

Git Flow를 이용하면 더 편리하게 브랜치 만들고 머지하고 PR을 보낼 수 있다. 

---
참고: 티스토리 - 탱크, 티스토리 - 안녕주의 코딩일기


