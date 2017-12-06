# Firebase Storage 사용해보기 (Download with Glide)
Firebase Storage는 사진, 동영상 등의 사용자 제작 콘텐츠를 저장하고 제공해야 하는
앱 개발자를 위한 서비스입니다. 저의 경우, 사진을 저장할 서버가 필요했었는데
서버를 구축하는 비용을 줄이고자 Firebase Storage를 사용하게 되었습니다.

제가 개발할 앱에서는 인증을 사용하지 않고 다운로드만 필요로 했기 때문에
Storage에서 보안 규칙을 공개로 설정하는 방법과 업로드한 사진을 앱에서 다운로드 하는
방법에 대해 정리하고자 합니다.


## 프로젝트 생성
Firebase 기능을 사용하기 위해서는 프로젝트 등록이 필요합니다.
[Firebase 콘솔](https://console.firebase.google.com) 로 이동해서 `StorageTest`
라는 이름으로 프로젝트를 추가합니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/1.png?raw=true)

프로젝트를 추가하면 프로젝트 설정을 위한 화면으로 이동하게 됩니다.
이동해서 보니,,, 어랏!!! 언제 리뉴얼 됐는지 모르겠지만 디자인이 확 바뀌었습니다.
부분 유료 전환도 하고 콘솔도 리뉴얼을 했나봅니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/2.png?raw=true)


## Storage 활성화
생성한 프로젝트에 Storage 기능을 설정해보겠습니다.
먼저, 왼쪽 사이드 메뉴에서 Develop/Storage 를 선택하고 기능을 사용하기 위해서 `시작하기` 버튼을 누릅니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/3.png?raw=true)

그러면 다음과 같이 저장소가 생성됩니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/4.png?raw=true)

생성된 저장소에 `photos` 라는 폴더를 만들고 로컬에 있던 이미지 하나를 골라 업로드를 하겠습니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/5.png?raw=true)

여기까지 저장소를 생성했고 다운로드 할 파일은 업로드 되었습니다.

## Storage 보안 규칙 변경
앞에서 설명한 것처럼 인증없이 다운로드 할 수 있어야 하기 때문에 보안 규칙을 공개로 변경하겠습니다.
'Firebase Console > StorageTest > Develop > Storage' 로 이동해서 `규칙` 탭을 선택합니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/6.png?raw=true)

여기서 규칙에 대한 설정을 다음과 같이 변경합니다.

```javascript
service firebase.storage {
  match /b/{bucket}/o {
    match /{allPaths=**} {
      allow read, write;
    }
  }
}
```

내용을 변경하게 되면 위에 `게시`와 `삭제`라는 버튼이 보이게 됩니다. 내용 변경 후, `게시` 버튼을 선택합니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/7.png?raw=true)

성공적으로 게시를 하게 되면 다음처럼 '보안 규칙이 공개로 설정되어 있으므로 모든 사용자가 저장소 버킷을 읽거나 쓸 수 있습니다'
라는 경고 메시지가 출력됩니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/8.png?raw=true)

Storage 설정은 끝났습니다. 이제 이 Firebase 프로젝트를 앱에 추가해야 합니다.
앱은 새로 만들지 않고 Firebase 에서 제공하는 [샘플](https://github.com/firebase/FirebaseUI-Android) 를
다운받아 사용하겠습니다. 다운로드 받아 Android Studio 에서 Open 합니다.

## 앱 추가
Firebase 프로젝트에 앱을 추가해야 하므로 Firebase Project Overview 화면으로 이동합니다.
그리고 `Android 앱에 Firebase 추가`를 선택합니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/2.png?raw=true)

위 Github 에서 다운받은 안드로이드 프로젝트의 패키지명은 `com.firebase.uidemo` 입니다.
이 패키지명으로 앱을 추가합니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/9.png?raw=true)

앱을 등록하면 `google-services.json` 파일을 다운로드 받을 수 있습니다. 화면에 표시되는 가이드대로
프로젝트의 `app` 폴더 아래에 다운로드한 `google-services.json` 파일을 복사합니다.
샘플프로젝트에는 dependencies 설정이 되어있기 때문에 `SDK 추가`를 하지않고 완료합니다.

## 샘플 프로젝트 실행
이 상태에서 샘플 프로젝트를 바로 실행하면 Storage 기능을 사용할 수 있습니다.
앱의 메인 화면에서 `Storage Image Demo` 선택하면 Storage 의 Upload와 Download 기능을 사용해볼 수 있습니다.

`Storage Image Demo` 처음에 인증 실패 토스트가 표시되지만 보안규칙을 공개로 했기 때문에
상관없이 사용할 수 있습니다. `Choose Image` 버튼을 선택해서 사진을 선택헤 Upload 합니다.
Upload 에 성공하면 Firebase 콘솔 화면에서 아래와 같이 추가된 파일을 볼 수 있습니다.

![make project](https://github.com/amuyu/TIL/blob/master/blog/img/firebase_storage/10.png?raw=true)

그리고 앱에서는 Download 버튼이 활성화되는데 `DOWNLOAD` 버튼을 선택하면 업로드한 사진을 ImageView
에서 볼수 있습니다.

## Download with Glide
이제 코드를 수정해서 아까 직접 업로드한 image를 ImageView에 표시해보겠습니다.
두 가지 파일을 수정해야 합니다. `ImageActivity.java`와 `activity_image.xml` 파일을 엽니다.

먼저, `activity_image.xml` 에서 `R.id.button_download_direct` enabled 설정을 true 로 변경합니다.

```xml
<Button
    android:id="@+id/button_download_direct"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:enabled="true"
    android:text="@string/download" />
```

그리고 `R.id.first_image` visible 설정을 visible로 변경합니다.

```xml
<ImageView
        android:id="@+id/first_image"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:contentDescription="@string/accessibility_downloaded_image"
        android:scaleType="centerCrop"
        android:src="#E6E6E6"
        android:visibility="visible"
        tools:visibility="visible" />
```

그 다음 `ImageActivity` 를 수정합니다. DOWNLOAD 버튼을 눌렀을 때, 업로드한 Image가 아니라
업로드한 파일을 가져오도록 변경합니다. `R.id.button_download_direct` 의 클릭 이벤트 메소드로
이동해 다음과 같이 수정합니다.

```java
@OnClick(R.id.button_download_direct)
protected void downloadDirect() {
    StorageReference ref = FirebaseStorage.getInstance().getReference("photos/road.png");
    // Download directly from StorageReference using Glide
    // (See MyAppGlideModule for Loader registration)
    GlideApp.with(this)
            .load(ref)
            .centerCrop()
            .transition(DrawableTransitionOptions.withCrossFade())
            .into(mImageView);
}
```

수정 후, 앱을 다시 실행해서 `Storage Image Demo` 화면으로 이동해 `DOWNLOAD` 버튼을 선택하면
처음에 로컬에서 업로드한 이미지를 ImageView 에서 볼 수 있습니다.

사진/동영상 등의 컨텐츠 업로드/다운로드 및 접근할 때 인증 기능 또한 제공하기 때문에
Firebase Storage를 사용하면 이와 같은 기능을 사용하는 서비스를 쉽게 구현할 수 있을 듯 합니다.
다만 유료로 변경되어 가격적인 부분에 대한 고려가 필요할 것 같습니다.

## Gradle 설정 및 AppGlideModule
Firebase Storage와 Glide를 사용해 ImageView 에 표시하기 위해
필요한 dependencies 설정은 다음과 같습니다.

### google-service
google-services 를 사용하기 위한 dependencies 를 추가합니다.
```groovy
// build.gradle
dependencies {
  ...
  // Google Services Gradle Plugin
  classpath "com.google.gms:google-services:3.1.0"
  ...
}
// app/build.gradle
apply plugin: 'com.google.gms.google-services'
```

### glide & firebase storage
glide 와 firebase storage 를 사용하기 위한 dependencies 를 추가합니다.
```groovy
// app/build.gradle
dependencies {
  ...
  // glide
  implementation 'com.github.bumptech.glide:glide:4.4.0'
  annotationProcessor 'com.github.bumptech.glide:compiler:4.4.0'

  // FirebaseUI Storage only'
  implementation 'com.firebaseui:firebase-ui-storage:3.1.0'
  implementation 'com.google.firebase:firebase-firestore:11.4.2'
  ...
}
```

### AppGlideModule
Glide 에서 `StorageReference` 를 사용해 image를 load 할 수 있도록
다음 클래스를 추가합니다.
```java
@GlideModule
public class MyAppGildeModule extends AppGlideModule {

    @Override
    public void registerComponents(Context context, Glide glide, Registry registry) {
        Logger.d("");
        registry.append(StorageReference.class, InputStream.class, new FirebaseImageLoader.Factory());
    }
}
```
