## Gradle 사용하기
### Gradle 사용
1.설치 
  - 파일 다운로드 [그레이들](http://www.gradle.org)
  - 특정 디렉토리에 복사  

2.그레이들 설치 경로를 환경 변수 등록
3.테스트로 build.gradle 파일 생성
```gradle
task helloworld << {
  println 'hello dowon'
}
```
4.테스트 수행
```bash
$ gradle -q helloworld
```
### 웹 애플리케이션 빌드
자바 프로젝트를 자동 생성하고, 이를 확장해서 웹 애플리케이션을 빌드하는 방법과 톰캣에서 실행하는 방법 소개
#### 웹 프로젝트 작성
자바 프로제트를 생성한다
```bash
$ gradle init -type java-library
```
build.gradle 편집
자바 소스 추가
#### 웹 애플리케이션 실행
Tomcat 플러그인이 제공하는 내장 톰캣을 이용하므로 실행 환경에 톰캣을 따로 설치할 필요가 없다
```bash
$ gradle tomcatRunWar
```
#### WAR 파일 생성
```bash
gradle war
```
war 태스크를 실행하면 build/libs 디렉토리에 WAR 파일이 생성된다.


