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
git pull
```
원격 저장소의 변경 내용이 로컬 작업 디렉토리에 받아지고(fetch), 병합(merge) 된다.
다른 가지의 내용을 현재 가지에 병합하려면 아래 명령을 실행한다.
```bash
git merge <가지 이름>
```
충돌이 발생하면 git이 알려주는 파일의 충돌 부분을 직접 수정해서 병합이 가능하도록 해야 한다. 충돌을 해결했다면 git에게 수정한 파일을 병합하라고 알려준다.
```bash
git add <파일 이름>
```
변경 내용을 병합하기 전에, 어떻게 바뀌었는지 비교할 수 있다.
```bash
git diff <원래 가지> <비교 대상 가지>
```

### 꼬리표 달기
소프트웨어 새 버전을 발표할 때마다 꼬리표를 달아놓으면 좋다
```bash
git tag 1.0.0 <식별자>
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

### merge와 rebase 차이
Git 매뉴얼 정의 내용은 다음과 같다
- git-merge : join two or more development histories together.
- git-rebase : Forward-port local commits to the updated upstream head

무엇을 쓰는게 좋은가? 히스토리 관리를 별로 신경쓰지 않고 혼자서만 커밋하거나 아니면
믿을 수 있는 소수만 커밋하지 않는다면 merge 대신 rebase를 사용하는 것이 좋다.

## 참고
- [git간편안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [git merge와 rebase 비교하기](https://blog.outsider.ne.kr/666)
- [svn 대비 git의 차별점](http://seungzzang.blogspot.kr/2013/04/git-svn-svn-git.html)