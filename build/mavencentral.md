## maven central link
1. [issues.sonatype.org/projects/OSSRH ](https://issues.sonatype.org/projects/OSSRH) 에 가입하고 이슈를 create 한다.
2. issue.sonatype.org에 가입한 정보를 bintray에 등록
3. 패키지 페이지의 `Mave Central` 탭으로 이동
4. token password 는 OSSRH의 패스워드를 입력
5. 입력 후, `SYNC` 버튼 클릭
6. Sync status 가 Successfully 로 업데이트 되면 성공
7. [search.maven.org](https://search.maven.org) 에 접속해서 프로젝트 등록되었는지 확인

## issue
### override version
release 한 버전은 변경이 안된다. 새로운 버전 release
https://issues.sonatype.org/browse/OSSRH-34782

### add new user
새로운 유저 추가는 groupid 에 댓글을 단다.
https://issues.sonatype.org/browse/OSSRH-13276

# 참고
[nexus manager](https://oss.sonatype.org/#stagingRepositories)
[repo browser](http://repo2.maven.org/maven2/)
