---
title: "apk가 빌드되는 과정 - signing key, upload key, certificate"
tags:
  - android
---

안드로이드 스튜디오에서 assembleDebug, bundleRelease 등의 커맨드로 apk/aab 파일이 만들어지면 보통 그 결과물 그대로 마켓에 올리던지 설치하던지 하는데,
이 과정에서 gradle이 자동으로 해주는 과정들이 있다.
바로 **zipAlign과 signing**인데, build.gradle에 buildType에 따라 signingConfig를 지정해주고,
또 디폴트로 debug일때는 zipAlign이 false, release일때는 true로 설정되기 때문에 이러한 과정들이 자동으로 진행된다.


만약 gradle에 위의 설정들이 없었다고 가정하면, **결과물로 만들어지는 순수한 apk는 코드와 리소스를 한데 모은 압축 파일일 뿐이다**.
이 순수한 결과물을 흔히 **unsigned apk**라고 한다.

release일 경우 위 산출물에 **zipAlign**이라는 동작을 가하는데, zipAlign은 **파일들의 byte alignment를 맞춰 앱이 사용하는 RAM을 줄여주게 된다**(고 한다).

마지막으로 **signing**이라는 과정을 통해 **이 산출물이 누구의 소유물인지를 파일에 기록**해둔다. signing이 끝난 결과물을 **signed apk**라고 한다.

signing 관련해서 keystore, signing key, upload key, certificate 이라는 개념들이 나오는데, 간단히 정리하자면:

- **keystore**: .keystore 혹은 .jks의 확장자를 가진 파일로, **하나의 keystore에서 여러 개의 서로 다른 key를 발급**할 수 있다.
    - keystore내의 각각 다른 key들은 alias라는 이름으로 구분하는데, alias는 단어 뜻 그대로 '별명'으로, **2개의 서로 다른 keystore에서 같은 alias로 key를 발급한다고 해서 두 개의 key가 동일한건 아니다**.
- **signing key와 upload key**: 두 개의 key는 구별하기 쉽도록 **개념적으로만 구분지었을 뿐이지, 발급받는 방식이 다르다던가 key의 형태가 다르다던가 하는건 없다**.
    - 둘 다 keystore를 통해 똑같이 발급받는 key인데, 플레이스토어에서 key 관리 정책을 시작하면서 두 개념을 구분하기 시작했다.
    - **signing key는 유저의 단말에 실제로 설치될 apk에 서명하기 위해 사용되는 key**이다.
    - **upload key는 플레이스토어에 앱을 등록할때만 사용되는 key**이다.
    - signing key와 upload key를 따로 쓰고 있다면, **플레이스토어에서 upload key로 된 서명을 벗겨낸 후 signing key로 다시 서명하는 과정을 진행**한다.
    - signing key와 upload key를 동일하게 사용할 수도 있는데, 그럼 벗겨나고 다시 입히는 과정은 생략된다 (구글에서 보안상의 이유로 key를 따로 쓰는 것을 권장하긴 한다).
- **certificate**: .der 혹은 .pem의 확장자를 가진 파일로, **각각의 key에 대응하는 certificate가 존재**한다.
    - certificate은 **서명에 사용된 key의 진위성을 판별할 수 있는 public key와 extra identifying information을 담고 있다**.
    - 플레이스토어에 upload key를 등록할 때 해당 key의 certificate를 등록하게 되는데, 그럼 apk/aab를 등록할 때 **플레이스토어에서는 저장된 certificate를 사용하여 해당 파일이 자신이 알고 있는 key로 서명된건지 확인**한다.
    - 마찬가지로, 사용자의 단말에 이미 설치된 apk가 새로운 버전으로 업데이트 될 때도, **새로운 apk가 동일한 key로 서명된 파일인지 확인하는 절차를 거치는데, 단말에 저장해둔 certificate로 새로운 apk의 서명을 검증**한다.
    - certificate는 단순히 key의 검증 용도로만 사용되기 때문에 **누구에게나 공유 가능**하며, **외부 api를 사용할 때 certificate을 등록해달라는 말이 signing key에 대응하는 certificate를 등록해달라는 말**이다.
