## Gradle 사용하기
### Gradle 핵심 기능
- 전형적인 반복 작업을 자동화하기 위한 구조
- 자동화된 처리를 실행하기 위한 부품


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

#### 그레이들 데몬
그레이들 데몬을 사용하면 JVM을 상주시키기 때문에 매번 JVM을 실행하거나 정지하지 않아도 되서 명령 실행에 걸리는 시간을 줄여준다.

그레디을 데몬의 사용법은 매우 간단하다. gradle 명령 실행 시 옵션 --daemon을 지정하면 끝이다.
```bash
$ gradle --daemon hello
```
그레이들 데몬은 일정 시간이 지나면 자동으로 종료된다. 명시적으로 종료하고 싶다면 다음 명령을 실행한다.
```bash
gradle --stop
```

#### 그레이들 래퍼
그레이들 래퍼를 이용하면 개발자들이 직접 그레이들을 설치하지 않아도 빌드할 수 있다.
빌드는 다음 순서를 거쳐서 그레이들의 바이너리를 자동으로 내려받고 바로 빌드가 진행된다.
1. 서브버전이나 깃 같은 저장소에서 프로젝트를 체크아웃한다.
2. gradlew 명령을 실행한다.

프로젝트 폴더로 돌아가서 그레이들 래퍼를 테스트 해보자.
```bash
$ gradle wrapper
```
명령을 실행하면 gradlew, gradlew.bat, gradle-wrapper.jar, gradle-wrapper.properties
를 자동으로 생성한다.
파일|내용
---|---
gradle/wrapper/gradle-wrapper.jar|그레이들 래퍼의 부트스트랩
gradle/wrapper/gradle-wrapper.properties|그래이들 래퍼의 설정 파일
gradlew|그레이들 래퍼 실행용 셀 스크립트
gradlew.bat|그레이들 래퍼 실행용 배치 파일(윈도용)
그레이들 래퍼를 사용할 때는 gradle이 아닌 gradlew를 사용해야 한다.
그레이들 래퍼는 그레이들 사용자의 설치 수고를 줄여주며, 다음과 같은 이점이 있다.
- 사용할 그레이들 버전을 고정할 수 있다. 그레이들의 버전 차이에 따른 빌드 오류를 방지할 수 있다.
- 젠킨스 같은 CI 툴 실행 환경에 그레이들을 설치하지 않아도 된다. CI 환경을 구축하는데 드는 수고를 덜 수 있다.

### 자바 프로젝트 빌드
#### Java 플러그인이란
Java 플러그인은 자바 빌드에 필요한 '태스크', 설정을 단순화하는 '규칙' 그리고 그것을 구현하기 위한 '속성' 및 '소스 세트'가 패키지로 구성된 빌드 기능 컴포넌트다.

