# 파일 시스템으로서 git
svn 과 같은 기존의 vcs 는 통상 새로운 버전을 저장할 때 변경이 발생한 파일에서 변경된 부분을 저장한다.
그러나 Git은 변경이 발생하지 않은 파일은 이전 파일을 그대로 참조하고 변경이 발생한 파일은 통째로
다시 저장한다. 결과적으로 하나의 버전은 한 벌의 소스 파일을 모두 가지고 있다.
그래소 Git은 snapshot 방식으로 이력을 관리한다고 얘기한다.

버전별로 온전한 한 벌의 파일이 있기 때문에 버전 간 이동 시 해당 파일을 그대로 불러오기만
하면 된다. 이러한 장점은 브랜치 관리에도 적용된다. Git의 브랜치는 그저 특정 버전을 가리키는
포인터일 뿐이므로 브랜치 간 이동은 곧 버전 간 이동일 뿐이다.

# Git 저장소
Git 저장소는 git init 명령어로 저장소를 생성하거나 git clone 명령어로 원격의 저장소를
복제했을 때 생긴다. 프로젝트를 관리하는 디렉터리 아래의 .git 디렉터리가 Git 저장소다.

저장소를 생성했을 때 기본으로 만들어지는 .git 디렉토리와 하위 디렉토리의 구조다.
dir : hooks, info, objects, refs
file : config, desription, HEAD

## Bare 저장소
작업 디렉토리가 없는 저장소를 Bare 저장소라고 하며 주로 중앙 저장소로 사용한다.
Git 도 공유나 배포, 버전 관리를 위해 중앙 저장소를 운용할 수 있다.
이 중앙 저장소에서는 소스의 관리와 공유만이 목적이므로 직접 내용을 수정할 필요가 없다.

bare 저장소 생성 명령어
```sh
git init --bare
```
일반 저장소의 .git 디렉토리를 복사해서 디렉토리 이름을 바꿔도 bare 저장소가 된다.
`myproject.git` 과 같이 디렉토리 이름에 .git 을 붙이는 형태로 변경한다.

## objects 디렉터리
objects 디렉터리는 파일이 저장되는 디렉터리다. Git은 모든 파일을 BLOB 객체로 저장한다.
파일을 저장할 때 SHA-1 로 체크섬을 계산한다. 체크섬을 계산한 해시값의 첫 두 자리를
디렉터리 이름으로 사용하고 나머지를 파일 이름으로 사용한다.

objects 디렉터리의 info 디렉터리와 pack 디렉터리에는 메타 정보와 packfile 이 저장된다.
packfile은 스냅숏 방식으로 저장한 파일을 변경분 저장 방식으로 바꿔 하나의 파일로 묶어 저장한 파일이다.
packfile 은 관리하는 파일이 많아지거나 작업 디렉터리의 파일을 원격 저장소로 푸시할 때 자동으로
만들어지며 `git gc` 명령어를 실행했을 때도 만들어진다.

objects 디렉터리에 객체가 생성되는 시점은 작업 디렉터리에 파일을 저장하는 시점이 아니다.
git add 명령어로 Staging Aread 에 파일을 등록할 때 생성된다.

objects 디렉터리에는 이 외에도 파일 목록을 담은 트리 객체와 커밋 정보를 담은 커밋 객체를 저장한다.
트리 객체는 다른 트리 객체를 하위에 가질 수 있다. 커밋 객체는 특정 최상위 트리 객체와 저자 등의
메타 정보 및 부모 커밋의 정보를 가진다.

최초로 커밋한 커밋 객체에는 연결 정보가 없지만, 파일을 수정하고 커밋해 새로운 커밋 객체를 만들면
이전 커밋에 대한 정보가 커밋 객체에 포함된다. 다음과 같이 새로운 커밋 객체의 정보를 보면
이전 커밋을 가리키는 부모 커밋 아이디(parent)가 있다.

```sh
$ git commit -m "modify file_a.txt "
[master d022f72] file_a.txt is modified
 1 file changed, 1 insertion(+)

$ git log master --pretty=format:"%H %P"
d022f729321216a7c7cc1a33c2a6971b29a78a70    17793cb158d198d62d82991dcdd557e28d23fb0  
17793cb158d198d62d82991dcdd557e28d23fb04

$ git cat-file -p d022f729321216a7c7cc1a33c2a6971b29a78a70
tree 011f70daf08a116f78ae744ed651f3acb1c79edd  
parent 17793cb158d198d62d82991dcdd557e28d23fb04  
author gerrit000 <gerrit000@naver.com> 1460288741 +0900  
committer gerrit000 <gerrit000@naver.com> 1460288741 +0900

modify file_a.txt 
```

## refs 디렉터리
Git 의 커밋 아이디는 파일 이름과 마찬가지로 SHA-1 값을 사용한다.
이 값은 길고 복잡해서 직접 커밋 아이디를 이용하기에는 불편하다.
따라서 특별한 의미가 있고 사용 빈도가 높은 커밋은 별도의 이름을 지정해서 사용하는데
이를 레퍼런스라 한다. 레퍼런스가 가리키는 커밋 아이디는 refs 디렉터리 아래에 
레퍼런스별 파일로 저장된다. 이 파일의 경로를 레퍼런스로 사용한다.

예를 들어 기본으로 존재하는 마스터 브랜치는 refs/heads/master 파일에 마지막
커밋 아이디가 저장되며 이 상대 경로가 그대로 레퍼런스로 사용된다. 
브랜치나 태그의 경우에는 좀 더 간편한 방법을 제공한다. 
상대 경로를 모두 사용할 필요 없이 브랜치나 태그의 이름만 사용해도 된다.

로컬에서 checkout 명령으로 branch를 생성하면 refs 에 추가된다.
```sh
git checkout -b temp
// .get/refs/heads/temp 가 추가됨
```

다음은 git log 명령어로 RB-1.0.0 이라는 브랜치에 연결되는 커밋과 그 부모 커밋 아이디를 출력한 예시다.
```sh
git log RB-1.0.0 --pretty=format:"%H %P"
```

레퍼런스는 사용자가 직접 생성해서 사용할 수 있다. Gerrit은 별도로 생성한 레퍼런스에 
리뷰와 관련된 정보를 저장한다. 다음과 같이 인위적으로 만든 refs/my/reference 파일에
RB-1.0.0 커밋 아이디인 a16cda62a1c4a33d8c01014e3c921455c1413968 을 저장하고
이 레퍼런스의 로그를 출력하면 해당 커밋부터 이전 로그가 동일하게 나오는 것을 볼 수 있다.

```sh
$ mkdir .git/refs/my
$ echo a16cda62a1c4a33d8c01014e3c921455c1413968 > .git/refs/my/reference
$ git log refs/my/reference --pretty=format:"%H %P"
a16cda62a1c4a33d8c01014e3c921455c1413968    443b3ca8b63e7bd2dc5ef61bc9e6a3baaf86e24  
443b3ca8b63e7bd2dc5ef61bc9e6a3baaf86e24d    d9a7feb6c439654d5c5f1ea04541708672ab32b  
d9a7feb6c439654d5c5f1ea04541708672ab32b5 
```

## info 디렉터리
info 디렉터리에는 Git 관리 제외 대상 목록을 저장한 exclude 파일과 레퍼런스 목록을
저장한 refs 파일이 있다.

exclude 파일은 작업 디렉터리의 .gitignore 파일과 기능은 동일하지만 다른 사람과 공유되지 않는다.
다른 사람과 공유할 필요가 없는 파일이나 디렉터리를 지정하고 제외 목록 자체도 공유할 필요가 없을 때 유용하다.

.git/info/refs 파일은 git gc 명령어를 실행하면 생성된다. 기존 .git/refs 디렉터리 아래의
개별 레퍼런스 파일을 모두 지우고 내용을 하나의 파일로 정리한다.

## hooks 디렉터리
hooks 디렉터리에는 훅 스크립트 파일이 저장된다. 훅 스크립트는 특정한 Git 명령어가 실행될 때
자동으로 실행되는 명령어의 묶음이다. 예를 들어 Gerrit이 제공하는 commit-msg 훅 스크립트는
git commit 명령어를 실행하면 change-id 를 자동으로 생성해 커밋 메시지에 포함한다.

## config 파일
.git 디렉터리의 config 파일은 파일이 있는 저장소에 적용되는 설정 파일이다.
Git은 시스템 설정 파일, 사용자 설정 파일, 로컬 설정 파일 3가지 설정 파일을 사용한다.

시스템 설정 파일은 git 설치 디렉터리에 있는 /etc/gitconfig 파일이다.
시스템 설정 파일의 내용은 git을 설치한 시스템 전체에 적용된다.

사용자 설정 파일은 사용자 디렉터리에 있는 .gitconfig 파일이다. 사용자 설정 파일에 
설정된 값은 특정 사용자가 사용하는 모든 Git 저장소에 공통으로 적용된다.

로컬 설정 파일은 그 파일이 있는 저장소에만 적용되며 시스템 설정 파일과 사용자 설정 파일의
내용을 대체할 수 있다. .git 디렉터리의 config 파일이 로컬 설정 파일이다.
config 파일에는 주로 해당 Git 저장소의 관리 정책, 원격 저장소와의 연결 정보 및 레퍼런스 매핑을
담고 있다.

예를 들어 git clone 명령어로 원격 저장소를 복제해 저장소를 만들면 다음 예와 같이 접속 정보가 설정된
설정 config 파일이 생성된다.

```config
[core]
    repositoryformatversion = 0
    filemode = false
    bare = false
    logallrefupdates = true
    symlinks = false
    ignorecase = true
    hideDotFiles = dotGitOnly
[remote "origin"]
    url = ssh://gerrit000@gerrit.navercorp.com:29418/remoteproject
    fetch = +refs/heads/*:refs/remotes/origin/*
[branch "master"]
    remote = origin
    merge = refs/heads/master
```

## HEAD 파일
.git 디렉터리의 HEAD 파일은 HEAD 레퍼런스가 가리키는 커밋 아이디를 저장한 파일이다.
HEAD 레퍼런스는 가장 특별한 레퍼런스로 항상 현재 작업 디렉터리에 체크아웃된 버전을 가리킨다.
만약 브랜치를 체크아웃하면 HEAD 파일에는 이 브랜치의 레퍼런스가 저장된다.

만약 커밋을 직접 체크아웃하면 detached HEAD 상태가 되며 이 때 HEAD 파일에는 해당 커밋 아이디가 저장된다.
detached HEAD 상태에서는 작업 디렉터리가 어떤 브랜치와도 연결되어 있지 않으므로 어떤 작업을 하든
기존 브랜치에 영향을 주지 않는다.

# ref
[Gerrit을 이용한 코드 리뷰 시스템 - Gerrit과 Git](https://d2.naver.com/helloworld/1859580)