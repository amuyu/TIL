## basic task
```groovy
task taskName {
    doLast {
        action
    }
}
```

## Copy Task
```groovy
task copy(type: Copy) {
  from "src"
  into "dest"
}

task ('copy', type:Copy) {
    from "src"
    into "dest"
}

task copy(type: Copy, group: "Custom", description: "Copies sources to the dest directory") {
    from "src"
    into "dest"
}
```

## Zip Task
```groovy
task zip(type: Zip, group: "Archive", description: "Archives sources in a zip file") {
    from "src"
    setArchiveName "basic-demo-1.0.zip"
}
```

## Custom Task
Custom
```groovy
class HelloTask extends DefaultTask {
    String message

    @TaskAction
    void print() {
        println message
    }
}

task hello(type: HelloTask) {
    message 'helloWorld'
}
```



[tutorial_tasks](https://docs.gradle.org/current/userguide/tutorial_using_tasks.html)
[custom-task](https://www.baeldung.com/gradle-custom-task)
[example](https://github.com/gradle/oreilly-gradle-book-examples)
