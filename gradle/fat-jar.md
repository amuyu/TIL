
fat jar 는 shadowjar 를 사용해서 만들었었는데 
gradle document 에 다음과 같은 내용이 있다.
shadowjar 는 사용하지 않아도 되나?
```groovy
task uberJar(type: Jar) {
    // 'uber' (AKA 'fat') 
    archiveClassifier = 'uber'

    from sourceSets.main.output

    dependsOn configurations.runtimeClasspath
    from {
        configurations.runtimeClasspath.findAll { it.name.endsWith('jar') }.collect { zipTree(it) }
    }
}
```


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