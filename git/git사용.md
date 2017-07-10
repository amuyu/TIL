## git 사용하기
### git 저장소 만들기
```bash
git init
```
### 저장소 받아오기
#### 로컬 저장소 복제
```bash
git clone /로컬/저장소/경로
```
#### 원격 저장소 복제
```bash
git clone 사용자명@호스트:/원격/저장소/경로
```
#### 작업의 흐름
1. Working directory : 실제 파일들
2. Index : 준비 영역(staging area)
3. HEAD : 최종 확정본(commit)
4. 원격 서버

### 추가와 확정
변경된 파일은 인덱스에 추가할 수 있다.
```bash
git add <파일 이름>
```
변경된 내용을 확정하려면 아래 명령을 실행한다.
```bash
git commit -m "이번 확정본에 대한 설명"
```
변경된 파일이 HEAD에 반영됨

### 변경 내용 발행(push)하기
현재 변경 내용은 아직 로컬 저장소의 HEAD안에 있는 상태
변경 내용을 원격 서버에 올린다. 아래 명령을 실행한다.
```bash
git push origin master
```
원격 저장소를 복제한 것이 아니라면 원격 서버의 주소를 git에 추가
```bash
git remote add origin <원격 서버 주소>
```
원격 저장소 변경
```bash
$ git remote -v
 	# View existing remotes
origin  https://github.com/user/repo.git (fetch)
origin  https://github.com/user/repo.git (push)

$ git remote set-url origin https://github.com/user/repo2.git
# Change the 'origin' remote's URL
```

### 가지치기
안전하게 격리된 상태에서 만들 때 사용한다
저장소를 새로 만들면 기본으로 master 가지가 생성된다.
이제 다른 가지를 이용해서 개발을 진행하고, 나중에 개발이 완료되면
master 가지로 돌아와 병합한다.
아래 명령으로 "feature_x"라는 이름의 가지를 만든다.
```bash
git checkout -b feature_x
```
아래 명령으로 master 가지로 돌아올 수 있다.
git checkout master
아래 명령으로 가지를 삭제할 수 있다.
git branch -d feature_x
새로 만든 가지를 원격 저장소로 전송하기 전까지는 다른 사람이 접근할 수 없다.
git push origin <가지 이름>

### 갱신과 병합
로컬 저장소를 원격 저장소에 맞춰 갱신하려면 아래 명령을 실행한다.
```bash
git pull	// fetch + merge
```
원격 저장소의 변경 내용이 로컬 작업 디렉토리에 받아지고(fetch), 병합(merge) 된다.
다른 가지의 내용을 현재 가지에 병합하려면 아래 명령을 실행한다.
```bash
git merge <가지 이름>
git merge -Xtheirs <가지 이름>
git merge -Xours <가지 이름>
```
충돌이 발생하면 git이 알려주는 파일의 충돌 부분을 직접 수정해서 병합이 가능하도록 해야 한다. 충돌을 해결했다면 git에게 수정한 파일을 병합하라고 알려준다.
```bash
git add <파일 이름>
```
변경 내용을 병합하기 전에, 어떻게 바뀌었는지 비교할 수 있다.
```bash
git diff <원래 가지> <비교 대상 가지>
```
### 원격 저장소 가져오기 정리 Fetch
git pull 보다 git fetch를 사용하면 직접 병합 작업을 할 수 있다.
```bash
git fetch					// 최신 커밋 이력 가져오기
git status					// 상태 확인
git branch -a 				// branch  확인
git merge origin/master		// 병합
git diff					// 차이점 확인
// 수정
git add <파일이름>
git commit <파일이름>
git push origin master
```

### 꼬리표 달기
소프트웨어 새 버전을 발표할 때마다 꼬리표를 달아놓으면 좋다
```bash
git tag 1.0.0 <식별자>
```
### 태그 공유하기
git push 명령은 자동으로 리모트 서버에 태그를 전송하지 않는다. 태그를 만들었으면 서버에 별도로 Push해야 한다.
```bash
git push origin <tag>
```
### 태그 삭제하기
'-d' 옵션 사용
```bash
git tag -d <tag>
```
### 원격 저장소 태그 삭제하기
':'를 사용하여 삭제
```bash
git push origin :<tag>
```


### 로컬 변경 내용 되돌리기
실수로 무언가 잘못한 경우, 로컬의 변경 내용을 되돌릴 수 있다.
```bash
git checkout -- <파일 이름>
```
로컬의 변경 내용을 변경 전 상태(HEAD)로 되돌려준다.
다만, 이미 인덱스에 추가된 변경 내용과 새로 생성한 파일은 그대로 남는다.
만약, 로컬에 있는 모든 변경 내용과 확정본을 포기하려면,
아래 명령으로 원격 저장소의 최신 이력을 가져오고,
로컬 master 가지가 저 이력을 가리키도록 할 수 있다.
```bash
git fetch origin
git reset --hard origin/master
```

### 유용한 팁
- git의 내장 GUI : gitk
- 콘솔에서 git output 을 컬러로 출력하기 : git config color, ui, true
- 이력(log)에서 확정본 1개를 딱 한 줄로만 표시하기 : git config format.pretty oneline
- 파일을 추가할 때 대화식으로 추가하기

### merge와 rebase
merge와 rebase는 Git에서 한 브랜치에서 다른 브랜치로 합치기 위해 사용한다
#### merge와 rebase 차이
Git 매뉴얼 정의 내용은 다음과 같다
- git-merge : join two or more development histories together.
- git-rebase : Forward-port local commits to the updated upstream head

무엇을 쓰는게 좋은가?
Merget를 사용할 때,
히스토리 관리를 별로 신경쓰지 않고 혼자(소수)서만 커밋할 때, ?? 확인이 필요함

rebase의 Log는 히스토리가 선형적으로 쌓인다. 모든 작업이 차례대로 수행된 것처럼 깔끔하게 보인다.
rebase는 자신의 커밋 히스토리를 이쁘게 만들기 위해 쓰는게 좋다.

### Merge 예
실제 개발과정에서 겪을 만한 예제를 하나 살펴보자. 브랜치와 Merge는 보통 이런 식으로 진행한다
1. 작업 중인 웹사이트가 있다.
2. 새로운 이슈를 처리할 새 Branch를 하나 생성.
3. 새로 만든 Branch에서 작업 중.  

이때 중요한 문제가 생겨서 그것을 해결하는 Hotfix를 먼저 만들어야 한다. 그러면 다음과 같이 할 수 있다:
1. 새로운 이슈를 처리하기 이전의 운영(Production) 브랜치로 이동.
2. Hotfix 브랜치를 새로 하나 생성.
3. 수정한 Hotfix 테스트를 마치고 운영 브랜치로 Merge.
4. 다시 작업하던 브랜치로 옮겨가서 하던 일 진행.  

```bash
# issue 작업
git checkout -b iss53
git commit #issue 53

# hotfix 작업
git checkout master
git checkout -b hotfix
git commit #hotfix

# hotfix merge
git checkout master
git merge hotfix # Fast-forword merge

# 필요없는 branch 삭제
git brach -d hotfix

# 하던 issue 계속 작업
git checkout iss53

# 53번 merge
git checkout master
git merge iss53 # Git 은 각 브랜치가 가리키는 커밋 두 개와 공통 조상 하나를 사용하여 3-way Merge를 한다.

# 충돌이 발생했을 때
git status # 충돌한 파일 확인해서 파일을 직접 수정한다
git add # 다시 Git에 저장
git status # 충돌이 해결됐는지 확인

# 충돌 취소하기
git merge --abort # 충돌하기 전으로 돌아감
Merge를 처음부터 다시 하고 싶다면
git reset --hard HEAD

# 공백 무시하기
git merge -Xignore-space-change whitespace

# 필요없는 branch 삭제
git branch -d iss53


```

### 로컬 저장소를 원격 저장소에 추가 하기
```bash
git remote add origin <저장소>
git push origin master
```

## 수정한 파일 임시 저장
```bash
// 저장
git stash
// list 확인
git stash list
//  최근 stash  적용
git stash apply
//  최근 strash  적용, staged 상태까지 복원
git stash apply --index
// strash 적용 후 바로 스택에서 제거
git stash pop
// 제거
git stash drop
```

## 원복
```bash
// untracked 파일 제거
git clean -f
git clean -f -d // 디렉토리 까지 제거
```

## commit 변경
```bash
git commit --amend
```

## 참고
- [git간편안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [git merge와 rebase 비교하기](https://blog.outsider.ne.kr/666)
- [svn 대비 git의 차별점](http://seungzzang.blogspot.kr/2013/04/git-svn-svn-git.html)
- [Git브랜치-Rebase하기](https://git-scm.com/book/ko/v1/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase%ED%95%98%EA%B8%B0)
- [초심자를 위한 github 협업 튜토리얼](https://blog.weirdx.io/post/45529)
