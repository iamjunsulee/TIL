# Git과 Svn

### 형상관리, 버전관리

보통 프로젝트를 진행하면 혼자하는 경우보단 여러 명과 협업하는 경우가 많다. 

각자 맡은 부분을 개발을 하게 되는데, 각자가 개발한 코드 혹은 문서들을 하나의 관리도구에서 통합적으로 관리하는 것을 __소프트웨어 형상 관리(SCM, Software Configuration Management)__라고 한다.

소프트웨어 소스 코드만을 관리하는 것을 버전관리라 하고, 형상관리보다 좁은 개념이다. 그러나 일반적으로 버전 관리, 소스 관리, 형상 관리 모두 동일한 정보에 대한 여러 버전을 관리한다는 뜻으로 같이 쓰이는 것 같다.



### 버전 관리 시스템

Git, SVN, CVS, Mercurial 등 많은 버전 관리 시스템이 존재한다.

그 중 가장 대표적인 git과 svn에 대해서 얘기해보자.

__git과 svn의 어떻게 다를까?__

1. __로컬 저장소와 원격 저장소의 분리__
   svn은 보통 저장소가 서버에 있다.(중앙 집중식)
   git은 저장소가 내 컴퓨터(로컬)에 있다.(분산 관리식)
   그렇다면 git은 다른 사람과 협업을 어떻게 할까? 원격 저장소를 만들면 된다.
   로컬 저장소에 작업 내용을 commit하고, 다른 사람들과 공유하기 위해 원격 저장소에 push하면 된다.

   _로컬 저장소가 있으면 어떤 장점이 있을까?_

   - 인터넷을 경유할 필요가 없으니 빠르다.
   - 내 로컬 저장소에서 마음껏 테스트를 하면 되니까 commit에 부담이 없다.
   - 원격 저장소와 연결이 끊겨도 계속 버전관리가 가능하다.
   - 원격 저장소가 문제가 생겨도 로컬 저장소로 복원이 가능하다.

2. __Staging Area 존재__
   git에는 로컬 저장소에 commit하기 전, 변경한 파일들을 Staging Area에 추가하는 단계가 하나 더 있다.
   (물론 git commit -a 명령어를 통해 바로 커밋을 할 수 있다.) 
   여러 파일을 수정했다고 가정하자. 하지만 특정 파일 하나는 커밋을 하면 안되는 상황이라면?
   svn에서는 해당 파일을 복사해서 백업해놓고 다시 revert 시킨 후에 전체 commit을 한 후, 다시 백업 파일을 복구할 것이다. 반면 git에서는 Staging Area에 특정 파일 하나는 빼놓고 나머지만 올리면 된다. 

3. __스냅샷(snapshot)을 이용한 버전 관리__
   svn은 파일의 변화(차이점)를 저장한다.
   git은 그 순간의 스냅샷으로 저장한다.
   특정 버전 A를 가져온다고 한다면 svn은 기초가 되는 파일과 함께 모든 변경 내역을 서버에서 내려받는다. 반면 git은 네트워크를 거치지 않고, 가장 가까운 스냅샷들만으로 특정 버전을 빠르게 만들어낸다.

4. __Branch__
   svn은 디렉토리 구조이다. 작업자가 브랜치를 만들면 서버에 저장된 변경 내역들을 순차 적용하여 실제 파일들을 만들어낸다. 전체 파일을 네트워크를 통해 통째로 내려받기 때문에 느리다.
   git은 서버에 이런 논리적인 디렉토리 구조를 가지지 않는다. 연속된 스냅샷이 순차적으로 이어지다가 가지를 치면서 브랜치가 만들어진다. 브랜치 이동과 병합이 svn에 비해 빠르고 간단하다.



차이만 본다면 svn 대신에 git을 무조건 사용해야만 할 것 같다.

그러나 브랜치나 태그 기능을 사용하지 않는다거나 이전 버전에 접근하는 데 딱히 불편함을 못 느낀다거나 svn 서버에 문제가 생긴 적이 없다면 굳이 잘 쓰고 있는 svn을 git으로 바꿀 필요까진 없는 것 같다.








