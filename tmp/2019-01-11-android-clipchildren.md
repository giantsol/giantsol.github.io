---
title: "안드로이드 View의 clipChildren 속성"
tags:
  - android
---

보통 View는 자신의 canvas 바깥 영역에 그리지 못한다. 하지만 clipChildren이라는 속성을 조정하면 바깥 영역에도 그릴 수 있게 된다. 아래 영상을 보자:

![image is clipped]({{ site.url }}{{ site.baseurl }}/assets/images/clipchildren_mov01.gif){: .align-center}

노란색 FrameLayout안에 검정색 FrameLayout이 있고, 그 안에 이미지가 있는 단순한 레이아웃이다. 이 상태에서 이미지를 위로 translate 시킨다. clipChildren을 디폴트 값인 true로 그대로 두면 그림이 검정이의 영역을 벗어나면서 잘리는 것을 볼 수 있다. 현재 xml은 아래와 같이 생겼다:

```xml
<!-- 노랑이 -->
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F8E24C">

    <!-- 검정이 -->
    <FrameLayout
        android:layout_width="400dp"
        android:layout_height="400dp"
        android:background="#371F1F"
        android:layout_gravity="center">

        <ImageView
            android:id="@+id/image"
            android:layout_width="200dp"
            android:layout_height="wrap_content"
            android:adjustViewBounds="true"
            android:layout_gravity="center"
            android:src="@drawable/jelly"/>

    </FrameLayout>

</FrameLayout>
```

이제 최상단 노랑이 FrameLayout에 ```android:clipChildren="false"```값을 줘보자. 아래와 같이 동작이 바뀐다:

![image is not clipped]({{ site.url }}{{ site.baseurl }}/assets/images/clipchildren_mov02.gif){: .align-center}

검정이 밖으로 이미지가 translate되어도 안잘리고 그려진다!

원리는 이렇다. 안드로이드의 View는 자신을 canvas에 그릴 때, 자신에게 주어진 영역만큼 clip 한 후 그린다. 그래서 clip된 바깥의 영역에는 아무리 그리려고 해도 그리지 못하는게 디폴트 동작이다.

<캔버스가 전승되는 다이어그램>

여기서 노랑이에게 clipChildren 값을 false로 주면 **"나의 children들은 canvas 안자르고 그대로 사용하도록 해!"**라고 명령하는 것이다. 여기서 children이란 직속 children만 해당한다. 즉, 노랑이 안의 검정이에게는 효력을 발휘하고, 검정이 안의 이미지에게는 효력을 발휘하지 않는다. 이 값을 바꾸면 canvas의 전승 과정이 아래와 같이 바뀐다:

<캔버스가 전승되는 다이어그램2>
