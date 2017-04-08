# export jar
```gradle
// This is the actual solution, as in http://stackoverflow.com/a/19037807/1002054
task clearJar(type: Delete) {
    delete 'build/libs/opencv.jar'
}
task makeJar(type: Copy) {
    from('build/intermediates/bundles/release/')
    into('build/libs/')
    include('classes.jar')
    rename('classes.jar', 'opencv.jar')
}
makeJar {}.dependsOn(clearJar, build)

dependencies {
}
```


# 안드로이드 라이브러리 만들기
[Android Library Project 만들어서 jar 파일로 추출하기](http://eunplay.tistory.com/54)
[Android Studio에서 나만의 Library 만들어보기](http://flowarc.tistory.com/entry/Android-Studio에서-나만의-Library-만들어보기)
[Android Studio Library Import 방법](http://dwfox.tistory.com/31)
[Android Studio AAR 파일 만들기](http://blog.burt.pe.kr/android-studio-aar-파일-만들기-33/)
[How to add .aar dependency in library module?](http://stackoverflow.com/questions/34765190/how-to-add-aar-dependency-in-library-module)
[How to export library to Jar in Android Studio?](http://stackoverflow.com/questions/16763090/how-to-export-library-to-jar-in-android-studio)
[android studio에서 jar 만들기](https://dotkebi.blogspot.kr/2016/02/android-studio-jar.html)



# 참고

[Providing the SDK as an aar archive](https://docs.onegini.com/msp/android-sdk/5.04.01/topics/setting-up-the-project.html#providing-the-sdk-as-an-aar-archive)
[how do I include a local jar from a dependent java project in an Android build?](http://stackoverflow.com/questions/22360737/gradle-how-do-i-include-a-local-jar-from-a-dependent-java-project-in-an-android/22415260#22415260)
