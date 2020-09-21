---
title: "안드로이드의 아이콘 뱃지"
tags:
  - android
---

앱에 새 알림이 오면 아이콘 옆에 자그만 똥글뱅이나 숫자가 달리는데 그걸 **아이콘 뱃지**라고 한다.
원래는 Android 8.0부터 지원되는 기능이지만, 삼성같은 단말에선 8.0 미만에서도 자기들끼리 만들어서 쓰고 있었다고 한다.
그래서 Pixel, Nexus같은 **구글 레퍼런스 폰**들은 8.0 미만에서 뱃지가 없을텐데, 삼성, LG 단말에서는 뜨는 단말이 있을 것이다.

**8.0 미만 단말에서 뱃지를 사용하는 방식**은 아래와 같다:

```kotlin
Intent("android.intent.action.BADGE_COUNT_UPDATE")
    .putExtra("badge_count", badgeCount)
    .putExtra("badge_count_package_name", packageName)
    .putExtra("badge_count_class_name", "com.tmp.MyActivity")
    .run { context.sendBroadcast(this) }

if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.N) {
    Intent("com.sec.intent.action.BADGE_COUNT_UPDATE")
        .putExtra("badge_count", badgeCount)
        .putExtra("badge_count_package_name", packageName)
        .putExtra("badge_count_class_name", "com.tmp.MyActivity")
        .run { context.sendBroadcast(this) }
}
```

첫번째 `android.intent.action.BADGE_COUNT_UPDATE`는 다른 블로그에서도 쓰이는걸 많이 볼 수 있는데,
그 아래에 있는 `com.sec.intent.action.BADGE_COUNT_UPDATE`는 Android 7.0에서만 특별히 필요한 경우인가보다 ([참고](https://support.citrix.com/article/CTX227296)).

한편, (Android가 공식적으로 뱃지를 지원하는) **8.0 이상부터는 Notification 설정에서 뱃지를 조절하기 때문에 위 방식의 효과가 사라진다** (사실 혼용하면 좀 이상하게 쓰인다).
`NotificationChannel`을 생성할 때 `setBadge(Boolean)`을 통해 해당 채널에 속한 Notification이 도착했을 때 뱃지를 표현할 것인가 안할 것인가를 정한다.
또한, `NotificationCompat.Builder`에서 `setCount(Int)`를 통해 뱃지에 표현될 숫자를 변경할 수 있다.

삼성 단말 8.0 이상에서 Notification과 위의 Broadcast를 동시에 사용하면 조금 이상한 현상이 목격된다:

1. `setBadge(false)`인 Notification이 왔을 때는 저 Broadcast를 보내도 뱃지가 표시되지 않는다.
2. `setBadge(true)`인 Notification이 달린 상태에서 badgeCount++를 하면서 Broadcast를 날리면 뱃지에 달린 숫자가 업데이트된다.
**하지만, `badgeCount = 0`을 하고 Broadcast를 보내도 Notification이 남아있으면 뱃지에 1 이상의 숫자가 달린 채로 남아있다**.

## 아이콘 뱃지 표시하기 정리:

1. 8.0 이상 단말에서는 `NotificationChannel`과 `NotificationCompat.Builder`의 설정을 통해서만 설정되도록 하는게 좋다.
2. 8.0 미만 단말 중 아이콘 뱃지를 지원하는 경우에는 위 Broadcast를 통해 설정되도록 한다.
