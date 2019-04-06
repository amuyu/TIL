# build란?
소스 코드 파일을 컴퓨터에서 실행할 수 있는 독립 소트프웨어 가공물로 변환하는
과정 또는 결과를 일컫는다.

다음은 소스 코드 언어의 종류에 따른 빌드 내용이다.
- c 언어의 유닉스 계열 라이브러리
컴파일 후 생성된 객체 파일을 링크해서 .a 파일이나 .so 파일로 변환
- 자바 어플리케이션
컴파일 후 .class 파일과 함께 META-INF 디렉터리와 매니페스트 파일(MANIFEST.MF)
을 추가해서 JAR 파일로 압축한다.

빌드에는 컴파일부터 결과물 생성까지 프로세스 외에도
- 지정한 디렉터리의 파일을 별도의 디렉토리에 복사한다.
- 지정한 애플리케이션을 실행한다.

같은 처리도 포함한다.

# 그레이들 빌드 실행
그레이들에서 빌드 실행은
스크립트 파일, properties 파일, 환경변수, buildSrc 등을 모아 실행한다.

# 빌드 생명 주기
초기화 단계 > 설정 단계 > 실행 단계로 나뉜다.
초기화 단계 : 스크립트 파일 읽기, 프로젝트 구성 판정, project 객체 생성, 속성 설정
설정 단계 : 의존 라이브러리 해석, Task 객체 생성, 태스크 그래프 작성
(* 태스크 그래프 : 태스크 간 의존관계를 그래프로 만듦)
실행 단계 : 태스크 그래프에서 실행 대상 태스크를 추출하여 실행한다.

# copy
- replication 디렉터리를 복사 위치로 지정한다.
- 모든 디렉터리에 있는 dummy.txt 는 제외한다.
- 복사 대상으로 original 디렉토리를 지정하지만, sub1과 sub2 디렉터리는 제외한다.
- replicaiton/internal 디렉터리에 original/sub1, original/sub2 디렉터리의 내용을 복사한다.

```groovy
copy {
  into 'replication'
  exclude '**/dummy.txt'

  from('original') {
    exclude 'sub1/', 'sub2/'
  }
  into('internal') {
    from 'original/sub1', 'original/sub2'
  }
}
```

# Log
logger 속성을 사용해 로그를 남길 수 있다.

# building kotlin-dsl
```
```
[build kotlin](https://guides.gradle.org/building-kotlin-jvm-libraries/)

# task
// https://docs.gradle.org/current/dsl/org.gradle.api.Task.html
task 는 project 에 속한다. TaskContainer 의 메소드를 사용할 수 있다.
build file 에 task
group, description 은 optional
```groovy
// kotlin-dsl?
task copy(type: Copy, group: "Custom", description: "Copies sources to the dest directory") {
    from "src"
    into "dest"
}
```

## properties task
빌드 스크립트에 정의된 속성 목록 표시
```sh
$ gradle propeties
./gradlew properties
```

# plugin
전형적인 반복 작업 자동화
자동화된 처리를 실행
## Java plugin
// https://docs.gradle.org/current/userguide/java_plugin.html

## Application plugin
빌드 스크립트에 메인 클래스를 지정, 그레이들에서 app 실행
app 실행 환경에 배포하기 위한 압축 파일 생성
```groovy
apply plugin: 'application'

mainClassName = 'com.example.cli.SimpleCalc'
applicationName = 'SimpleCalc'

dependencies {
  compile 'commons-cli:commons-cli:1.2'
}

// gradle에서 실행할 때 app이 표준 입력을 받을 수 있도록 설정
run {
  standardInput = System.in
}
```
Application plugin 을 적용하면 run 태스크가 추가되고
gradle에 의해 실행할 수 있다.

# shadowJar
kotlindsl 사용
```groovy
import com.github.jengelman.gradle.plugins.shadow.tasks.ShadowJar

plugins {
    id("com.github.johnrengelman.shadow") version "2.0.4"
}

val shadowTasks = tasks.withType<ShadowJar> {
    classifier = ""
    version = ""
    description = "Create a fat JAR of ${project.name} v$version and runtime dependencies."
    doLast {
        val size = archivePath.length()
        println("FatJar: ${archivePath.path} (${size})")
    }
}

tasks {
    "build" {
        dependsOn(shadowTasks)
    }
}
```


# Jar
```
task apiJar(type: Jar) {
    baseName "publishing-api"
    from sourceSets.main.output
    exclude '**/impl/**'
}
```

## sourceJar
```
val sourcesJar by tasks.creating(Jar::class) {
    baseName = "${rootProject.name}-${project.name}"
    from(java.sourceSets["main"].allSource)
}
```

# multiple publishing
[multiple-publications](https://github.com/gradle/gradle/tree/master/subprojects/docs/src/samples/maven-publish/multiple-publications)
[onejar](https://stackoverflow.com/questions/13782013/can-gradle-jar-multiple-projects-into-one-jar?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa)
[fat jar](https://stackoverflow.com/a/43998029/6759520)

# SourceSet
[sourceset](https://docs.gradle.org/current/dsl/org.gradle.api.tasks.SourceSet.html)
