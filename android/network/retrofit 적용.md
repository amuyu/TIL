#Retrofit
Square 회사에서 만든 REST API 통신을 위한 라이브러리
## 장점
AsyncTask, HttpUrlConnection을 사용해서 직접 네트워크 통신 구현 시, 귀찮은 작업들을 안하고 편하게 사용할 수 있음
- 네트워크 통신 연결/해제
- 가져온 데이터 파싱
- Json 통신 Class 변환
- 각종 에러 처리

## 설정

```gradle
compile 'com.squareup.retrofit2:retrofit:2.1.0'
compile 'com.squareup.retrofit2:converter-gson:2.1.0' // gson 사용
compile 'com.squareup.retrofit2:adapter-rxjava:2.1.0' // rxjava와 연동
```

## Retrofit 사용 방법(예제)
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
List<리포> repos = service.listRepos("amuyu");
```  

## Retrofit2 사용 방법(예제)
### Retrofit2 클래스 생성
```java
Retrofit retrofit = new Retrofit.Builder()
    .baseUrl(HOST)
    .addConverterFactory(GsonConverterFactory.create())
    .build();
```
### 서비스 객체 생성
```java
public interface Cards {
    @GET("/api/card")
    Call<CardInfo> getCardsInfoList(@Query("category") String menu, @Query("page") int page);

Cards cards = retrofit.create(Cards.class);
}
```
### 비동기 실행
```java
Call<카드인포> data = cards.getCardsInfoList("trend", 0);
        data.enqueue(new Callback<CardInfo>() {
            @Override
            public void onResponse(Call<CardInfo> call, Response<CardInfo> response) {
                Log.d(TAG, "onResponse");

                String result = null;
                if(response.body() != null)
                {
                    result = response.body().toString();
                }

                Log.d(TAG, "result : " + result);
            }

            @Override
            public void onFailure(Call<CardInfo> call, Throwable t) {
                Log.d(TAG, "onFailure");
            }
        });
```
## Retrofit2 과 기존 소스 비교
Retrofit2를 사용하면 AsyncTask 및 url/http 연결 등의 코딩이 사라지면서 코드가 훨씬 간결해진다.
### 기존 소스
```java
new TaskCheckVersion().execute();	//버전 체크

class TaskCheckVersion extends AsyncTask<Void, Void, String>{    	
    	public TaskCheckVersion() {}    	
    	@Override
    	protected void onPreExecute() {
    		super.onPreExecute();
    	}    	
    	@Override
    	protected String doInBackground(Void... params) {
    		String result = null;    		
    		try {
    			CheckVersionApi checkVersionApi = ApiPool.get().getApi(CheckVersionApi.class);
    			result = checkVersionApi.getCheckVersion("0", getAppVersion(), android.os.Build.VERSION.RELEASE);
    		} catch (Exception e) {
    			Logger.error(e);
    		}        	
    		return result;
    	}    	
    	@Override
    	protected void onPostExecute(String result) {
    		super.onPostExecute(result);    		
    		try {
    			Logger.i("noco", "result : "+ result);    			
    			Gson gson = new Gson();
    			VersionCheckData data = gson.fromJson(result, VersionCheckData.class); 
    			Log.d(TAG, "result : " + result);    			
    		} catch (Exception e) {
    			Logger.error(e);
    		}    		
    		delayHnadler();
    	}
}

public String getCheckVersion(String type, String version, String os_version){
        Uri.Builder uri = new Uri.Builder()
                .scheme(SCHEME)
                .encodedAuthority(HOST)
                .appendPath("safenumber")
                .appendPath("v2")
                .appendPath("app")
                .appendPath("check")
                .appendQueryParameter("device_type", type)
                .appendQueryParameter("version", version)
                .appendQueryParameter("os_version", os_version);
        
        Logger.d(TAG, "create() uri:" + uri.build().toString());
        Request request = new Request.Builder()
                .url(uri.build().toString())
                .build();

        try {
            Response response = mHttpClient.newCall(request).execute();
            String body = response.body().string();
            Logger.d(TAG, "create() response.body:" + body);
            Logger.d(TAG, "create() response.code:" + response.code());
            return body;
        } catch (Exception e) {
        	Logger.error(e);
            throw new ApiException(0, e.toString());
        }
	}
```
### Retrofit2 적용
```java
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(SkpApiConsts.SKP_API_URL)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
VersionCheckApi service = retrofit.create(VersionCheckApi.class);
Call<VersionCheckData> data = service.retroTest("0", "1.0.0", android.os.Build.VERSION.RELEASE);
data.enqueue(new Callback<VersionCheckData>() {
    @Override
    public void onResponse(Call<VersionCheckData> call, Response<VersionCheckData> response) {
        Log.d(TAG, "onResponse");
        String result = response.body().toString();
        Log.d(TAG, "result : " + result);
    }
    @Override
    public void onFailure(Call<VersionCheckData> call, Throwable t) {
        Log.d(TAG, "onFailure");
    }
});

public interface VersionCheckApi {
@GET("/safenumber/v2/app/check")
public Call<VersionCheckData> retroTest(@Query("device_type") String device_type,
                                        @Query("version") String version,
                                        @Query("os_version") String os_version);
}
```
### RxJava 적용
```java
Retrofit retrofit = new Retrofit.Builder()
                .baseUrl(SkpApiConsts.SKP_API_URL)
                .addConverterFactory(GsonConverterFactory.create())
                .build();
requestPublisher = PublishSubject.create();
requestPublisher
        .observeOn(Schedulers.io())
        .subscribe((Void) -> retrofit.create(VersionCheckApi.class)
                        .networkCheck(cpn)
                        .observeOn(AndroidSchedulers.mainThread())
                        .subscribe(versionCheckData -> Log.d(TAG, "runNetworkCheck#onNext : " + versionCheckData.toString()),
                                Throwable::printStackTrace),
                Throwable::printStackTrace);
requestPublisher.onNext(null);

public interface VersionCheckApi {
@GET("/safenumber/v2/app/check")
public Observable<VersionCheckData> retroTest(@Query("device_type") String device_type,
                                        @Query("version") String version,
                                        @Query("os_version") String os_version);
}
```

## 참고
[Retrofit 공식 사이트](http://square.github.io/retrofit/)
[Retrofit2를 이용한 RestAPI 통신하기](http://falinrush.tistory.com/5)
[안드로이드 http통신 retrofit 2.0](http://mythinkg.blogspot.kr/2015/11/http-retrofit-20.html)
[Retrofit2과 함께하는 정말 쉬운 HTTP](https://realm.io/kr/news/droidcon-jake-wharton-simple-http-retrofit-2/)
[Retrofit2 + okhttp3 + Rxandroid 사용법](http://tiii.tistory.com/11)
