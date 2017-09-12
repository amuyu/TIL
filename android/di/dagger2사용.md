# Dagger2

## Gradle 설정
### Android.Gradle
```gradle
// Add Dagger dependencies
dependencies {
  compile 'com.google.dagger:dagger:2.x'
  annotationProcessor 'com.google.dagger:dagger-compiler:2.x'
}

// Dagger2 2.0.1
// project.gradle
classpath 'com.neenbedankt.gradle.plugins:android-apt:1.8'

// app.gradle
apply plugin: 'android-apt'

compile 'com.google.dagger:dagger:2.0.1'
apt 'com.google.dagger:dagger-compiler:2.0.1'
provided 'org.glassfish:javax.annotation:10.0-b28'
```

## element
@Inject — base annotation whereby the “dependency is requested”
@Module — classes which methods “provide dependencies”
@Provide — methods inside @Module, which “tell Dagger how we want to build and present a dependency“
@Component — bridge between @Inject and @Module
@Scope — enables to create global and local singletons
@Qualifier — if different objects of the same type are necessary

## 개발
### Module 생성
#### provide 추가
#### provide 호출 클래스에  @Inject 추가
### Component 생성
### @Inject 추가
### scope 사용
scope 내에서 동일한 인스턴스 사용
@Singleton 은 app scope 안에서 singleton 객체가 생성되어 주입된다.
@PerActivity activity life cycle 에서만 존재할 수 있는 객체 사용


# kotlin
```kotlin
@Module
class LottieListModule(private val lottieListActivity: LottieListActivity) {
    @Suppress("UNCHECKED_CAST")
    @Named("LottieListViewModel")
    @Provides fun viewModelProvider(lottieModel: LottieModel): ViewModelProvider =
        ViewModelProviders.of(lottieListActivity, object : ViewModelProvider.Factory {
            override fun <T : ViewModel?> create(modelClass: Class<T>?): T {
                return LottieListViewModel(lottieModel) as T
            }
        })

    @Provides fun lottieListViewModel(@Named("LottieListViewModel") viewModelProvider: ViewModelProvider): LottieListViewModel =
        viewModelProvider.get(LottieListViewModel::class.java)

}
```

## 참고
[Android 개발에서 Dagger2이용해보기.](http://drcarter.tistory.com/169)
[JakeWharton 발표자료](https://speakerdeck.com/jakewharton/dependency-injection-with-dagger-2-devoxx-2014)
[Dagger2 학습에 필요한 참고 자료](http://kunny.github.io/life/2016/06/06/dagger2_resources/)
[Android and dagger2 발표자료](http://www.slideshare.net/ssuser70b5b8/android-and-dagger2)
[google/dagger](https://github.com/google/dagger)
[Dagger Document](http://google.github.io/dagger/)
[A Complete Guide To Learn Dagger 2](https://blog.mindorks.com/a-complete-guide-to-learn-dagger-2-b4c7a570d99c)
[Dependency injection with Dagger 2 - Custom scopes](http://frogermcs.github.io/dependency-injection-with-dagger-2-custom-scopes/)
[Dagger 2. Part I. Basic principles, graph dependencies, scopes.](https://android.jlelse.eu/dagger-2-part-i-basic-principles-graph-dependencies-scopes-3dfd032ccd82)
[Dagger2에서 @Singleton scope및 custom scope annotation이용](http://drcarter.tistory.com/173)
[Dagger 의 커피머신 예제 들여다보기](http://kingorihouse.tumblr.com/post/97061100384/dagger의-커피머신-예제-들여다보기)
