daum과 naver api 를 사용해 책에 대해 검색하는 어플
databinding, kotlin, mvvm 을 base architecture 로 사용하고 있다.
multi module 로 구성한 내용에 대해서 살펴볼 수 있다.

# 01_before
dagger를 적용하지 않은 코드로 `02_after`와 비교해서 어떻게 변하는지 확인할 수 있다.

## module
`main-app`, `naver-search`, `duam-search`, `common-repository`
dependencies 를 갖는다.

### main-app
main-app 에서는 HomeActivity와 SearchActivity 를 이루어져 있다.

이 중, SearchActivity 는 text View 와 결과를 표시할 pager 영역을 갖는다.
사용자가 입력하는 내용을 받아 SearchQueryRepository 에 전달한다.
전달된 Query 는 각각 module 에서 api 를 호출해 화면에 결과를 보여준다.

### daum-search, naver-search
Daum 검색 결과를 보여주는 모듈이다.

`SearchQueryRepository` 의 searchQuery 로부터 query 를 받아 `DaumSearchRepository` 에서
query 로 daum-api 를 호출한다. api 호출 후, 받은 response를 data에 추가 한다.
그러면 databinding 으로  recyclerview 에 데이터가 업데이트 되게 되고 화면이 갱신된다.


# 02_after
`main-app`, `naver-search`, `daum-sarch`, `search`, `common-repository` dependencies 를 갖는다.
`01_before` 와 달라진게 있다면 search 모듈이 추가된 것과 dagger 를 사용하고 있다는 점이다.

## main-app
`AndroidInjector` 를 사용하고 있는데 잘 모르겠다는..@.@ > subcomponent 를 빌드하기 위해 사용한다.
Dagger 를 사용해 모듈을 추가하는데 AppComponent에서 `SearchBinder` 를 통해
`naver-search`, `daum-sarch`, `search`, `common-repository`
네가지 module 에 접근한다.




# common-module
## common-repository
SearchQueryRepository 라는 클래스를 갖고 검색할 query 를 입력받고 실행하는 인터페이스를 갖는다.

## common-style
화면에 리스트로 표시할 Adapter 와 BookItem 에 대한 클래스를 갖는다.



# multi module 구성하는 방법
Settings.gradle 에 include 추가
new module
move module

## tip
### @BindingAdapter 사용

## 참고
[dagger2-module](https://github.com/ZeroBrain/AndroidSamples/tree/master/dagger2-module)
[블로그 글](https://medium.com/@jsuch2362/multi-module-과-dagger2-8472492eaba3)
