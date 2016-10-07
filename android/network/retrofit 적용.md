#Retrofit
Square 회사에서 만든 REST API 통신을 위한 라이브러리
## 장점
AsyncTask, HttpUrlConnection을 사용해서 직접 네트워크 통신 구현 시, 귀찮은 작업들을 안하고 편하게 사용할 수 있음
- 네트워크 통신 연결/해제
- 가져온 데이터 파싱
- Json 통신 Class 변환
- 각종 에러 처리

## 사용 방법(예제)
### Interface Clas 선언하기
user를 파라미터로 받아 API URL을 완성해서 GET 방식으로 요청
```java
public interface GitHubService {
    @GET("/users/{user}/repos")
    List<Repo> listRepos(@Path("user") String user);
} 
```
### Interface Class 구현하기
Retrofit에서 제공하는 restAdpater를 이용해서 Base URL(https://api.github.com)을 지정해주고 GitHubService 변수를 만들어준다.
```java
RestAdapter restAdapter = new RestAdapter.Builder()
        .setEndpoint("https://api.github.com")
        .build();
GitHubService service = restAdapter.create(GitHubService.class);
```
### Repo 클래스 만들기
json을 받아줄 클래스를 생성 json키에 해당하는 값들을 변수 이름으로 지정
```java
public class Repo {
    int id;
    String name;
    String full_name;
    
    @get,set
    ...
}
```
### 서비스 호출해서 리스트 가져오기
Interface Class의 함수를 사용하면 Repo클래스들의 리스트 형태를 결과로 받을 수 있다.
```java
List<Repo> repos = service.listRepos("amuyu");
```  


## 참고
[유용한 라이브러리 - Retrofit(REST API 통신)](http://gun0912.tistory.com/30)
[Retrofit에서 Interceptor를 이요해 쿠키/세션을 유지하는 방법](http://gun0912.tistory.com/50)