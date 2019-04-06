# DSL(Domain Specific Language)
도메인에 특화된 언어?
gradle 은 groovy 와 kotlin DSL 를 제공하는데
좀더 간단하게 스크립트를 작성할 수 있도록 도와준다.
에를 들어, 다음과 같이 작성된 스크립트는
```groovy
repositories {
    jcenter()
}
```
실제 코드로 변경하면 이런 코드다.
```groovy
def repositoryHandler = getRepositories()
repositoryHandler.jcenter()
```

# task
task는 `project`에 속해있다. TaskContainer 의 메소드를 사용할 수 있다.
```
task myTask
task myTask { configure closure }
task myTask(type: SomeType)
task myTask(type: SomeType) { configure closure }
```

# Task Actions
task는 특정 순서대로 실행되는 action을 포함한다.
task를 실행하면 각 action들이 실행된다.
task에 Task.doFirst, Task.doLast 를 호출해서 action을 추가할 수 있다.
```
// recommend
task hello {
    doLast {
        println 'Hello world!'
    }
}
// deprecated
task hello << {
    println 'Hello world!'
}
```


# Apply a plugin
다양한 플러그인 [gradle plugin portal](https://plugins.gradle.org/?_ga=2.54920427.1182041422.1530233984-950675478.1491201365)

base 플러그인을 사용하면 `Zip` task 와 연결되서 distribution 할 수 있다.
base 플러그인 없어도 되는데? base 플러그인이 없는 경우 build/distributions 폴더에 생성되지 않는다.
```
plugins {
    id("base")
}

tasks.create<Zip>("zip") {
    description = "Archives sources in a zip file"
    group = "Archive"

    from("src")
    setArchiveName("basic-demo-1.0.zip")
}
```


# create task
task 는 project 에 속한다. TaskContainer 의 메소드를 사용할 수 있다.
build file 에 task
group, description 은 optional
```groovy
// groovy
task copy(type: Copy, group: "Custom", description: "Copies sources to the dest directory") {
    from "src"
    into "dest"
}
// kotlin
tasks.create<Copy>("copy") {
    description = "Copies sources to the dest directory"
    group = "Custom"

    from("src")
    into("dest")
}
```

# 참고
[working with files](https://docs.gradle.org/current/userguide/working_with_files.html#sec:copying_directories_example)
