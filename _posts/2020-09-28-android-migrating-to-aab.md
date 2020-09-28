---
title: "안드로이드 apk에서 aab(앱번들)로 변경하기"
tags:
  - android
---

플레이스토어에 apk 형식으로 앱을 기재하면 aab(앱번들)를 사용하라는 워닝이 뜬다. 2021년 말인가? 즈음엔 aab로 강제한다고 했던거같은데.. (내 기억이 맞다면)
그래서 슬슬 aab로 갈아타야하는데, 기존에 apk로 올리던 앱들을 aab로 변경하는건 엄청 간단하다!

**aab를 사용하기 위해서는 Play App Signing이 필수**인데 ([이전 글](https://giantsol.github.io/android-app-bundle/) 참고), 이미 이 정책을 사용하고 있느냐 아니냐에 따라 조금 다르다.

## Play App Signing을 이미 사용하고 있던 경우

**아무런 설정도 변경 할 필요 없이**, 기존에 `./gradlew assembleRelease`로 마켓에 올릴 apk를 생성했다면, 이젠 `./gradlew bundleRelease`로 aab를 생성하면 된다.
생성된 aab 파일을 등록하면 된다.

끝! **마켓에 등록하는 파일만 달라질 뿐 겉으로는 달라지는게 없으며, apk 사이즈 축소라는 추가적인 장점**만 가지고 가게 된다.

## Play App Signing을 사용하지 않고 있던 경우

**우선 Play App Signing을 신청**해야 한다. 플레이 콘솔에서 앱 서명 페이지에 가면 signing key를 등록하고 upload key를 새로 발급받는 절차가 나온다.

1. **기존에 apk를 만들기 위해 사용하고 있던 release용 signing key를 등록**한다.
2. **플레이스토어에 앱을 등록하기 위한 upload key도 등록**한다.
    - **기존에 사용하던 signing key를 upload key로도 사용**할 수 있다.
    - 하지만, 더 철저한 보안을 위해 완전 **새로운 upload key를 만들어 사용하길 구글에서 권장**하긴 한다.
3. 이제 로컬에서 `./gradlew bundleRelease`를 통해 마켓에 등록할 aab 파일을 생성할 때, upload key를 사용하도록 gradle 설정을 변경한다.
    - **기존에 사용하던 signing key를 upload key로도 사용하게끔 설정했다면, 설정을 변경할 필요 없다**.
    - 반대로 새로운 upload key를 발급받았다면, **`bundleRelease`를 할 때만 upload key를 사용하고, 그 외의 빌드 환경에선 (e.g. `assembleRelease`) 기존의 signing key를 쓰도록 유지하는걸 추천**한다.
    - 마켓에 업로드할 aab 파일을 생성하기 위한 key만 변경되기 때문에, 다른 잠재적인 사이드이펙트가 일어날 일은 없다.
    
```groovy
signingConfigs {
    // ...
    release {
        storeFile = releaseKeyStoreFile
        keyAlias "keyAlias"
        storePassword("storePassword")
        keyPassword("keyPassword")
    }
}

// ...

releaseKeyStoreFile = {
    def taskRequest = gradle.startParameter.taskRequests.toString()
    if (taskRequest.contains("bundleRelease")) {
        return file("upload-key.jks")
    } else {
        return file("signing-key.jks")
    }
}()
```

4. `./gradlew bundleRelease`를 통해 aab 파일을 생성한 후, 마켓에 등록한다.

끝! 생각보다 간단하다!