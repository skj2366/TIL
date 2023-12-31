# Git Submodule

정원사들을 진행하면서 다른 분들의 깃허브도 보게되었는데, 흥미로운 Repository 활용방법을 발견해서 작성 🫡

바로 Repository에 다른 Repository를 커밋하는 느낌이랄까 ?
그래서 검색 해본 결과 서드파티인줄 알았는데 Git 공식문서에도 있는 기능이었다. 공부가 더욱 필요하다 🤣

### 서브모듈이란?

> **Git 저장소 안에 다른 Git 저장소를 디렉토리로 분리해 넣는 것**

다른 독립된 Git 저장소를 Clone 해서 내 Git 저장소 안에 포함할 수 있으며 각 저장소의 커밋은 독립적으로 관리한다.

### 서브모듈 시작하기

```
// 메인 저장소에 서브모듈을 추가하는 명령어
git submodule add {서브모듈 저장소의 URL}
```

-   추가하는 저장소의 URL은 절대경로도 되고 상대경로도 된다.
-   기본적으로 서브모듈은 프로젝트 저장소의 이름으로 디렉토리를 만든다.
    -   명령의 마지막에 원하는 이름을 넣어 다른 디렉토리 이름으로 서브모듈을 추가할 수도 있다.

위 명령어를 실행하게 되면 `.gitmodules` 파일이 생성된다.
이 파일은 서브디렉토리와 하위 프로젝트 URL의 매핑 정보를 담은 설정파일이다.

```
[submodule "{서브모듈 저장소의 이름}"]
    path = {서브모듈 저장소의 이름}
    url = {서브모듈 저장소의 URL}
```

-   서브모듈 개수만큼 이 항목이 생긴다.
-   .gitignore 파일처럼 버전을 관리
    -   Push & Pull
-   **gitmodules 파일에 있는 URL은 조건에 맞는 사람이면 누구든지 Clone 하고 Fetch 할 수 있도록 접근할 수 있어야 한다.**
    -   즉 private repository를 서브모듈로 사용 시, 상대경로로 사용하는 것이 좋다.

### 서브모듈 포함한 프로젝트 Clone

-   서브모듈을 포함하는 프로젝트를 Clone 하면 기본적으로 서브모듈 디렉토리는 빈 디렉토리이다.
-   서브모듈에 관련된 두 명령을 실행해야 완전히 Clone 과정이 끝난다.
-   `git submodule init`
    -   서브모듈 정보를 기반으로 로컬 환경설정 파일이 준비됨.
-   `git submodule update`
    -   서브모듈의 리모트 저장소에서 데이터를 가져오고 서브모듈을 포함한 프로젝트의 현재 스냅샷에서 Checkout 해야 할 커밋 정보를 가져와서 서브모듈 프로젝트에 대한 Checkout을 한다.
-   디렉토리는 마지막으로 커밋을 했던 상태로 복원된다.

#### 더 간단하게 클론 하는 방법

> git clone 명령 뒤에 --recurse-submodules 옵션을 붙이면 서브모듈을 자동으로 초기화하고 업데이트

```
  git clone --recurse-submodules {클론할 저장소 URL}
```

### 서브모듈 업데이트

`git fetch`

`git merge` 명령으로 Upstream 브랜치를 Merge

> 서브모듈을 최신으로 업데이트하는 더 쉬운 방법도 있다. 서브모듈 디렉토리에서 Fetch 명령과 Merge 명령을 실행하지 않아도 git submodule update --remote 명령을 실행하면 Git이 알아서 서브모듈 프로젝트를 Fetch 하고 업데이트한다.

## 정리

일단 여기까지가 나에게 필요한 부분인 것 같아서 여기까지 정리한다.

이 외에도 여러 기능과 옵션이 존재하는데 우선 서브모듈로 프로젝트를 생성하고 필요한 기능이 있으면 추가적으로 작성하는 것으로 하려고 한다.

> 출처 : https://git-scm.com/book/ko/v2/Git-%EB%8F%84%EA%B5%AC-%EC%84%9C%EB%B8%8C%EB%AA%A8%EB%93%88

## git submodule 삭제하기

kopring_tutorial 이라는 repository를 submodule로 사용중이었으나 제거해버린 상태인데 계속 서브모듈로 남아있어서 이참에 제거를 하려고 한다.

### `git submodule deinit -f` 명령어로 모듈을 deinit

-   e.g

```git
git submodule deinit -f kopring_tutorial
```

```
error: could not lock config file .git/modules/kopring_tutorial/config: No such file or directory
warning: Could not unset core.worktree setting in submodule 'kopring_tutorial'
Cleared directory 'kopring_tutorial'
```

> 나의 경우 미리 repo를 제거해둔 상태라서 이런 오류가 뜬 듯 하다. 그래도 Cleared direcory로 인하여 말끔하게 모든 내부 파일이 제거된 repo를 볼 수 있다.

### `.git/modules` 폴더에 들어가서 해당 폴더를 삭제

.e.g

```
rm -rf .git/modules/kopring_tutorial
```

### git에서 폴더 제거 (20년 이후로는 이 명령어만 해도 된다고 한다)

.e.g

```
git rm -f kopring_tutorial
```

### DATE

> 2023-11-06 23:15

### FIN
