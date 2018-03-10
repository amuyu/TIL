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
@Scope — enables to create global and `local singletons`
@Qualifier — if different objects of the same type are necessary, 의존성 식별이 용이하다.
@Resuable
@Releasable
@Binds - module 에서 제공하고자 하는 클래스의 구현체를 바인딩하고자 할 때 사용
@intomap - map 으로 multi binding 함

### Singleton 에 관해서
다음과 같은 모듈이 있다고 하자
```java
@Module
public class CafeModule {
    @Singleton
    @Provides
    CafeInfo provideCafeInfo(){
        return new CafeInfo();
    }

    @Provides
    CoffeeMaker provideCoffeeMaker(Heater heater){
        return new CoffeeMaker(heater);
    }

    @Provides
    Heater provideHeater(){
        return new Heater();
    }
}
```
cafeInfo를 제공하는 메서드에만 `@Singleton` 어노테이션을 사용했다.
singleton 을 사용한 cafeinfo는 component에서 객체 호출 시, 동일한 CafeInfo 객체가 호출된다.
하지만 sigletone을 적용하지 않은 coffemaker의 경우 호출 시, 매번 다른 coffeemaker가 호출된다.
```java
CafeComponent cafeComponent = DaggerCafeComponent.create();
CafeInfo cafeInfo1 = cafeComponent.cafeInfo();
CafeInfo cafeInfo2 = cafeComponent.cafeInfo();
// cafeInfo1 과 cafeInfo2 는 같다.
CoffeeMaker coffeeMaker1 = coffeeComponent1.coffeeMaker();
CoffeeMaker coffeeMaker2 = coffeeComponent1.coffeeMaker();
// coffeeMaker1 과 coffeeMaker2 는 다르다.
```

### Scope 적용에 관해서 (local scpoe)
다음과 같은 module 이 있다. component에는 `@CoffeeScope` 적용되어있다.
```java
@Module
public class CoffeeModule {

    @CoffeeScope
    @Provides
    CoffeeMaker provideCoffeeMaker(Heater heater){
        return new CoffeeMaker(heater);
    }

    @CoffeeScope
    @Provides
    Heater provideHeater(){
        return new Heater();
    }

    @Provides
    CoffeeBean provideCoffeeBean(){
        return new CoffeeBean();
    }

}
```
`@Singleton` 과 유사하게 component와 동일한 scope를 사용한 coffeemaker 와 heater 는
component 당 한개의 객체만 생성한다.


## injection
Lazy injection
Provider injection

## Subcomponent
부모 component 가 있는 component 로 일반 component 와는 달리 코드 생성은 부모 component 에서 이루어진다.
부모 Component에서 코드 생성이 이루어지기 때문에 SubComponent 는 특정 Component 에서만 접근할 수 있다.

SubComponent 를 생성하는 방법은 `@Subcomponent` annotation 을 주고 @Subcomponent.Builder 를 정의해주어야 한다.
그리고 정의한 Subcomponent를 Component에 encapsulate 해야 subcomponent와 component가 관계를 맺을 수 있다.
상위 Component에 적용된 모듈에 subcomponent를 적용시키면 component 끼리 상위-하위 관곌르 맺게 된다.

Module 에 sumponents를 적용하면 이 모듈을 가지고 있는 Component가 부모 component가 되고
부모 Component에서 subcomponent 를 생성해 사용할 수 있다.

## Component.builder
```java
public interface CafeComponent {

    CafeInfo cafeInfo();
    CoffeeComponent.Builder coffeeComponent();

    @Component.Builder
    interface Builder{
        Builder cafeModule(CafeModule cafeModule);
        CafeComponent build();
    }

}

CafeComponent cafeComponent = DaggerCafeComponent.builder()
        .cafeModule(new CafeModule("example cafe"))
        .build();
cafeComponent.cafeInfo().welcome();
```

## 개발
### Module 생성
#### provide 추가
#### provide 호출 클래스에  @Inject 추가
### Component 생성
### @Inject 추가
### scope 사용
해당 클래스의 단일 인스턴스가 존재하는 범위
Singleton은 같은 범위 내에서 하나의 인스턴스만을 반환한다.
@Singleton 을 사용하기 위해서 Scope 지정이 필요하다.
Component 와 provides 의 scope 는 동이하게 지정한다.
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
[DAGGER 2 - A New Type of dependency injection](https://www.youtube.com/watch?v=oK_XtfXPkqw&feature=youtu.be&t=16m58s)
[Dagger 2 - 기타 다른 Annotation ](https://brunch.co.kr/@oemilk/72)
[Di와 dagger2](https://cmcmcmcm.blog/2017/08/02/didependency-injection-와-dagger2-2/)
