
# copy task
copy 용 task
```groovy
android.applicationVariants.all { variant ->

    def task = project.tasks.create("copy${variant.name}Apk", Copy)
    task.from(variant.outputs[0].outputFile)
    task.into("${System.properties['user.home']}/Desktop/")
    def targetName = "${variant.baseName}-${variant.versionName}.apk"
    task.rename ".*", targetName
    task.doFirst {
        println "copy from ${source.singleFile.name} to $destinationDir"
    }

    task.doLast { value ->
        println "completed to copy : $targetName"
    }

}
```
