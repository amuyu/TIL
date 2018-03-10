# diagram
ui(f)>presenter(i)>usecases(b)>entities(d)
ui(f):Frameworks and Drivers
presenter(i):Interface Adapters (that convert data from the format most convenient for the use cases and entities,)
usecase(b):Application Business rules, business service, extension of the entities
entitiy(d):Enterprise Business rules, Domain logic or business object

# 개념이해
엔티티(Entities, 독립체): 서비스 비지니스 객체입니다.? 잘못된 해석..
유즈케이스(Use Cases, 사용 사례) 이 유즈케이스는 엔티티들 간에 데이터 흐름을 조정합니다. 인터렉터(Interactors, 소통자)라고도 합니다.
인터페이스 어뎁터(Interface Adapters, 인터페이스 각색자): 이 어뎁터 모음은 유스 케이스 및 엔티티에 가장 편리한 형식의 데이터를 변환합니다. 프리젠터들(Presenters)과 컨트롤러들(Controllers)이 여기에 해당합니다.
프레임워크 및 드라이버(Frameworks and Drivers): 여기에는 모든 세부 정보가 들어있습니다. UI, 도구, 프레임워크 등

# Domain 은 무엇인가?
모든 소프트웨어 프로그램은 그것을 사용하는 사용자의 어떤 활동이나 관심과 관계가 있다. 사용자가 프로그램을 적용하는 대상 영역이 소프트웨어의 도메인이다.

# Architectural approach
Presentation(Model View) - Domain(Regular Java) - Data(Repository)

# Architectural reactive approach
View > Presenter > Domain Use Case > Repository > Disk/Cloud
   subscriber         observable     observable
data direction <---------------------------------

# 궁금한거
mvvm 과 cleanarchitecture

# DDD
## Architecture
표현 > 응용 > 도메인 > 인프라스트럭쳐
표현 == UI, presenter
응용 == UseCase
도메인 == Entity
infra == repository (db, rest api)
## DIP
Dependency inversion principle, 의존 역전 원칙
고수준 모듈이 저수준 모듈에 의존하지 않도록 한다.
테스트를 용이하게 한다.

# 참고
[안드로이드의 MVC, MVP, MVVM 종합 안내서](https://news.realm.io/kr/news/eric-maxwell-mvc-mvp-and-mvvm-on-android/)
[Android Architecture - MVC에서 MVP에서 MVVM으로 가는 길](http://thdev.tech/androiddev/2017/08/09/Android-MVC_MVP_MVVM-Intro.html)
[클린 아키텍처를 안드로이드에 도입하는 방법](https://academy.realm.io/kr/posts/converting-an-app-to-use-clean-architecture/?)
[On the Journey from Legacy Code to Clean Architecture: Rebuilding the Buffer Android Composer](https://overflow.buffer.com/2016/12/22/rebuild-android-composer/)
  - buffer 에서 사용한 구조를 잘 설명해 놓음
[안드로이드를 설계하는 깨끗한 방법 - 번역](https://medium.com/@wickedev/지난-몇달-동안-동료들과-안드로이드에-대한-토론을-가진-이후-안드로이드-어플리케이션-아키텍처-에-대해서-아티클을-작성하기에-지금이-적기라고-판단했습니다-5ff61586fb43)
[The Clean Architecture - Unclebob](https://8thlight.com/blog/uncle-bob/2012/08/13/the-clean-architecture.html)
[안드로이드에서 Clean Architecture의 흔적과 장점](http://kimjihyok.info/2017/05/29/안드로이드에서-clean-architecture의-흔적과-장점/)
[Android Architecture: Part 3 – Applying clean architecture on Android](http://five.agency/android-architecture-part-3-applying-clean-architecture-android/)
[clean-architecture](https://github.com/mattia-battiston/clean-architecture-example)
