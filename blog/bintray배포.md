쉬운거 사용중 어려운거 안되서

## GPG Key 생성
maven central link 하기 위해서는 서명하기 위한 GPG Key 가 필요하다.

##3 maven central link
1. [issues.sonatype.org/projects/OSSRH ](https://issues.sonatype.org/projects/OSSRH) 에 가입하고 이슈를 create 한다.
2. issue.sonatype.org에 가입한 정보를 bintray에 등록
3. 패키지 페이지의 `Mave Central` 탭으로 이동
4. token password 는 OSSRH의 패스워드를 입력
5. 입력 후, `SYNC` 버튼 클릭
6. Sync status 가 Successfully 로 업데이트 되면 성공
7. [search.maven.org](https://search.maven.org) 에 접속해서 프로젝트 등록되었는지 확인


# 참고
[Android Module을 Bintray(JCenter)에 배포하는 방법](http://thdev.tech/androiddev/2016/09/01/Android-Bintray(JCenter)-Publish.html)
  - 어려운거
[Android lib jcenter로 배포하기](https://brunch.co.kr/@nser789/1)
  - 쉬운거
[Java 라이브러리 maven 저장소에 등록하기](http://jojoldu.tistory.com/161)
  - 설명 잘됨
[gradle 참고](https://github.com/Meisolsson/GitHubSdk/blob/master/library/build.gradle)
