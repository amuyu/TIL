
flavorSelection 'shape', 'square'

## productFlavors
제품 버전 구성
defaultConfig 와 동일한 속성을 지원하기 때문에 defaultConfig {} 블록에 있는 모든 버전에 대해 기본 구성을 제공할 수 있다.
```groovy
android {
    ...
    defaultConfig {...}
    buildTypes {...}
    productFlavors {
        demo {
            applicationIdSuffix ".demo"
            versionNameSuffix "-demo"
        }
        full {
            applicationIdSuffix ".full"
            versionNameSuffix "-full"
        }
    }
}
```
### 앱 이름 달리하기
manifestPlaceholders 를 이용한다.
```groovy
android {
    productFlavors {
        demo {
            ...
            manifestPlaceholders = [ appLabel: "Flavor(demo)"]
        }
        full {
            ...
            manifestPlaceholders = [ appLabel: "Flavor(full)"]
        }
    }
}
```
appLabel 설정하고 Manifest에 변수로 넣어주면 된다.
```xml
<application
    ...
    android:label="${appLabel}">
</application>
```
### 소스 달리하기
demo 와 full productFlavors가 추가되어 있는 상태에서 아래와 같이 설정 가능하다
```groovy
android {
    sourceSets {
        demo {
            manifest.srcFile 'src/demo/AndroidManifest.xml'
            java.srcDirs = ['src/demo/java']
            res.srcDirs = ['src/demo/res']
        }
        full {
            manifest.srcFile 'src/full/AndroidManifest.xml'
            java.srcDirs = ['src/full/java']
            res.srcDirs = ['src/full/res']
        }
    }
}
```

## flavorDimensions
'mode'와 'api'를 사용해서 여러 버전의 구성을 조합해보자
'mode'는 제품 버전을 그룹화하고, 'api'는 api레벨을 기반으로 그룹화 해보자
demo, full 에는 dimension 을 'mode'를 설정하고
minApi24, minApi23, minApi21 에는 dimension 을 'api'를 설정하자
```groovy
android {
  ...
  buildTypes {
    debug {...}
    release {...}
  }

  // Specifies the flavor dimensions you want to use. The order in which you
  // list each dimension determines its priority, from highest to lowest,
  // when Gradle merges variant sources and configurations. You must assign
  // each product flavor you configure to one of the flavor dimensions.
  flavorDimensions "api", "mode"

  productFlavors {
    demo {
      // Assigns this product flavor to the "mode" flavor dimension.
      dimension "mode"
      ...
    }

    full {
      dimension "mode"
      ...
    }

    // Configurations in the "api" product flavors override those in "mode"
    // flavors and the defaultConfig {} block. Gradle determines the priority
    // between flavor dimensions based on the order in which they appear next
    // to the flavorDimensions property above--the first dimension has a higher
    // priority than the second, and so on.
    minApi24 {
      dimension "api"
      minSdkVersion '24'
      // To ensure the target device receives the version of the app with
      // the highest compatible API level, assign version codes in increasing
      // value with API level. To learn more about assigning version codes to
      // support app updates and uploading to Google Play, read Multiple APK Support
      versionCode 30000 + android.defaultConfig.versionCode
      versionNameSuffix "-minApi24"
      ...
    }

    minApi23 {
      dimension "api"
      minSdkVersion '23'
      versionCode 20000  + android.defaultConfig.versionCode
      versionNameSuffix "-minApi23"
      ...
    }

    minApi21 {
      dimension "api"
      minSdkVersion '21'
      versionCode 10000  + android.defaultConfig.versionCode
      versionNameSuffix "-minApi21"
      ...
    }
  }
}
...
```
위와 같이 설정하면 'mode', 'api' 로 그룹이 되게 된다.
{demo, full} {minApi24, minApi23, minApi21}
Gradle은 빌드 구성에 맞춰서
demo-minApi24.apk 혹은 demo-minApi24.apk 등으로 각 빌드 유형을 조합해서 빌드 변형을 생성한다.

# 참고
[migration, flavor](https://developer.android.com/studio/preview/features/new-android-plugin-migration.html)
[Gradle로 여러가지 버전 생성하기](http://dktfrmaster.blogspot.kr/2016/10/gradle.html)
[productFlavors로 같은 소스코드 다른 앱 만들기](http://gun0912.tistory.com/74)
