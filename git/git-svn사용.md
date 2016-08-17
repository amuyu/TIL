## git-svn 사용하기
### git-svn이란?
svn을 원격저장소로하고 로컬에는 git을 핸들링하기 위한 프로그램
### 사용하는 이유?
개인적으로 작업을 할 때에 GIT이 편하고 유용한데 회사에서는 아직 SVN을 쓰고 있을 때,,
### 설치
[설치링크](https://git-scm.com/downloads) 에서 다운받아 설치한다
### git 로컬리포지토리 만들기
로컬에 리포지토리를 만든다
`git svn clone -s http://svn.server/path/project`
만약 svn 저장소가 기본 layout(i.e, trunk, branches, tags directory)로 구성이 되어 있지 않다면 `-s`를 빼고 명령을 호출한다.
`git svn clone http://svn.server/path/project`
### svn update 명령어
`git svn rebase`
### 원격 저장소 정보 불러오기
`git branch -r`
clone 한 이후에 만들어진 branch도 알기 위해서는 다음과 같이 실행한다
`git svn fetch svn`
### 로컬 브랜치 만들기
로컬 리포지트리에 local_brown을 만들고 원격 svn의 brown 브랜치를 보게 된다
`git checkout -b local_brown brown`
그 이후, 개발을 진행하면서 svn 원격저장소에 올리려면
`git svn dcommit`
svn의 commit을 읽어올 경우에는
`git svn rebase`
를 실행한다
### brown 리포지토리를 svn 리포지토리의 trunk로 merge하는 경우
원격저장소 svn 브랜치의 brown을 로컬 리포지토리의 master에 merge하여 원격 svn 리포지토리에 dcommit한다.
```git
git svn fetch svn
git checkout master
git merge brown --no-commit --no-ff
<conflict가 발생할 경우 여기서 수정>
git commit -m "merge from brown"
git svn dcommit
```

## 참고
- [git-svn의 구석구석](https://baepower.wordpress.com/2012/05/31/git-svn%EC%9D%98-%EA%B5%AC%EC%84%9D%EA%B5%AC%EC%84%9D/)
- [GIT와 SVN 연동하기 GIT-SVN](http://blog.cjred.net/177/)
- [svn 대비 git의 차별점](http://seungzzang.blogspot.kr/2013/04/git-svn-svn-git.html)
