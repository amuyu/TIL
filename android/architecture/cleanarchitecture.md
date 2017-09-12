# diagram
ui(f)>presenter(i)>usecases(b)>entities(d)
f:Frameworks and Drivers
i:Interface Adapters
b:Business rules
d:Domain logic

# Architectural approach
Presentation(Model View) - Domain(Regular Java) - Data(Repository)

# Architectural reactive approach
View > Presenter > Domain Use Case > Repository > Disk/Cloud
   subscriber         observable     observable
data direction <---------------------------------

# 궁금한거
mvvm 과 cleanarchitecture

# 참고
[안드로이드의 MVC, MVP, MVVM 종합 안내서](https://news.realm.io/kr/news/eric-maxwell-mvc-mvp-and-mvvm-on-android/)
[Android Architecture - MVC에서 MVP에서 MVVM으로 가는 길](http://thdev.tech/androiddev/2017/08/09/Android-MVC_MVP_MVVM-Intro.html)
