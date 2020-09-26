---
title: "안드로이드 App Bundle"
tags:
  - android
---


Android App Bundle (.aab 확장자)은 플레이스토어에 앱을 기재할 수 있는 새로운 방법이다. 앱을 기재할때는 크게 3가지의 방법이 있는데:

1. **Universal apk**: 그냥 하나의 apk 파일을 등록한다.
2. **Multiple apk**: apk 파일을 등록하는건 마찬가지이되 각 CPU, screen density 별로 다르게 빌드된 apk들을 여러개 올리는 방법이다 (e.g. app-v8a.apk, app-x86.apk).
단, **각 apk는 직접 빌드**해야한다.
3. **App Bundle**: aab 확장자를 가진 파일로, 기기에 앱이 최종 설치될 때 해당 기기의 CPU, screen density, 그리고 language를 고려하여 꼭 필요한 데이터만 다운받아질 수 있도록 하기 위한 새로운 방법이다.

그러면 App Bundle의 특징들을 쭉 살펴보자!

## App Bundle의 특징

1. Android 5.0 (Lollipop)부터 **split apk**라는 기능을 지원하는데, 쉽게 말해 **여러 개로 나뉘어진 작은 apk들을 하나의 앱으로 인식할 수 있는 기능**이다.
    - **App Bundle은 이 기능을 활용하는 방식**으로, **aab 파일 자체에는 앱의 모든 코드와 리소스가 들어가지만, 특정 기기에 설치될 때 꼭 필요한 작은 apk들만 분리되어 다운받아지는 방식**이다.
    - 플레이스토어에 aab 파일을 개시하면, **유저가 해당 앱을 다운받을 때 플레이스토어에서 aab로부터 apk들을 추출하고 signing key로 서명을 하는 과정**이 이루어진다.
    - 그럼 5.0 미만은?
        - 5.0 미만은 split apk를 지원하지 않기 때문에, 위의 multiple apk 방식으로 다운받아진다.
        - 즉, **개발자 입장에선 aab 파일 하나만 올리면 5.0 미만에서는 multiple apk로, 5.0 이상에서는 split apk로 다운**받아진다.
2. aab로 등록된 앱을 유저가 다운받을 때 **CPU, screen density, language로 구분된 apk들을 따로 다운**받게 된다.
    - e.g. Galaxy S6 Edge (arm64-v8a, xxxhdpi) 대상으로 aab에서 생성되는 apk 파일들:
        - base-master.apk
        - base-arm64_v8a.apk
        - base-xxxhdpi.apk
        - base-ko.apk
    - 사용자가 기기의 언어를 변경 시, **플레이스토어에서 새로운 언어에 해당하는 apk를 추가로 다운**받는다.
3. 로컬에서 apk 파일을 만들 때 ./gradlew assembleDebug를 하듯이, **aab 파일을 만들 때는 ./gradlew bundleDebug로 가능**하다.
    - 한편, ./gradlew bundleDebug를 할 시 aab 파일이 나오는데, 이 **aab에서 기기에 설치하기 위한 apk 파일들을 직접 추출하는건 좀 번거롭다** (bundletool이라는 툴이 쓰인다).
    - 대신, Android Studio 3.2 이상이라면 aab에서 apk를 설치하는 과정을 쉽게 할 수 있다.
        - Run > Edit Configurations에서 Deploy 옵션을 "**APK from app bundle**"로 바꿔준 후 실행한다.
4. 플레이스토어에 앱을 aab로 등록하기 위해서는 **Play App Signing이 필수**이다.
    - Play App Signing이란, **앱의 signing key를 구글에서 관리**하도록 기존에 쓰이던 signing key를 플레이스토어에 등록하는 것이다.
        - 필수인 이유는 **App Bundle의 경우 실제 apk를 만드는 과정이 구글쪽에서 진행되기 때문**이다 (apk를 추출하고 서명할 때를 위해).
        - App Bundle이 아닌 일반 apk를 사용할 때에도 Play App Signing을 이용할 수 있다.
    - Play App Signing을 이용할 때, **signing key와는 또다른 upload key를 요청**할 수 있다.
        - upload key는 **플레이스토어에 apk/aab를 등록할 때 쓰이는 key로, 로컬에서 apk/aab 파일을 빌드할 때** 쓰인다.
        - **기존에 사용하던 signing key를 upload key로도 사용하게끔 설정**할 수 있다. 단, 구글에서는 더 철저한 보안을 위해 서로 다른 key를 사용하도록 권장한다.
            - signing key와 upload key가 동일할 경우: **플레이스토어에 apk/aab를 등록하면 알맞은 signing key로 서명된건지 인증서(certificate)를 통해 확인**한다.
            - signing key와 upload key가 다를 경우: **알맞은 upload key로 서명된건지 인증서로 확인하고, 맞다면 해당 서명을 벗겨낸 후 등록된 signing key로 다시 서명**한다.
    - Play App Signing의 또다른 장점은 **key 분실에 대응할 수 있다는 점**이다.
        - 기존에 signing key를 upload key로도 사용하던 도중 signing key를 분실했을 경우: **새로운 upload key를 요청할 수 있고, 그 뒤 로컬에서 apk/aab를 빌드할 때 발급받은 upload key를 사용**하면 된다.
            - 유저에게 설치되는 apk의 서명은 (로컬에서는 분실되었지만) 구글에 등록된 signing key로 진행되기 때문에 유저 입장에서 업데이트에 지장이 없다.
        - 기존에 새로운 upload key를 사용하던 도중 upload key를 분실했을 경우: **위와 마찬가지로 새로운 upload key를 요청**하면 된다.
        - 단, **Play App Signing에 등록한 signing key 자체를 다시 받을 수는 없다**. 따라서, signing key를 분실한 경우:
            - upload key를 발급받으면 되니 플레이스토어에 앱을 기재하는 것에는 문제가 없으나,
            - 로컬에서 이전의 signing key로 빌드하는 것은 할 수 없게 된다는 뜻이며,
            - 플레이스토어가 아닌 다른 서드 파티 마켓에는 동일한 signing key로 업데이트 할 수 없어짐을 뜻한다.
            - 즉, **플레이스토어 앱 기재에는 문제 없으나 다른 상황들에선 문제가 생기는거니 signing key를 분실하지 말자**.
    - Play App Signing은 **앱 단위로 설정**된다.
5. Play Feature Delivery와 Play Asset Delivery를 지원한다.
    - Dynamic module을 지원한다는 뜻이다.
    - **Universal apk과 multiple apk에서는 이를 지원하지 못한다는 점이 차이점**이다.
6. 플레이스토어에 App Bundle 형태로 등록된 앱일 경우, 플레이스토어가 아닌 다른 경로로 기기에 apk를 설치했을 때 (즉 sideload 방식), **해당 apk의 versionCode가 플레이스토어에 등록된 aab의 versionCode와 동일하다면 자동 업데이트 on/off에 관계없이 플레이스토어로부터 apk를 새로 다운**받는다.
    - App Bundle 적용 후, 사용자들이 sideload 방식으로 apk를 주고받을 때 생길 수 있는 이슈들을 방지하기 위함이다.
