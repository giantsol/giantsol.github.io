---
title: "안드로이드 View의 clipChildren에 대하여"
excerpt_separator: "<!--more-->"
tags:
  - android
---

**기본적으로 안드로이드의 모든 view는 자신이 물리적으로 차지하는 영역 만큼만 그릴 수 있다**. 즉, 전체 화면을 기준으로 (left=100, top=100, right=200, bottom=200)만한 영역을 차지하는 view가 (left=200, top=200, right=300, bottom=300) 위치에 뭔가를 그리려고 하면 아예 아무것도 화면에 안나타날 것이다. 이는 모든 view가 부모 view의 canvas를 넘겨받고, 그 canvas를 자신의 영역 만큼만 clip해서 사용하기 때문이다. 이 clipping하는 과정으로 인해 자신이 차지하는 영역 밖으로는 아무것도 그리지 못한다.

하지만 자신의 영역 밖에 그려야 할 상황도 있다. 예컨대, 자신의 영역을 넘어서는 애니메이션을 돌려야 할 때. 이를 가능하게 해주는 속성이 [**clipChildren**](https://developer.android.com/reference/android/view/ViewGroup.html#attr_android:clipChildren)이다.

<!--more-->

이 속성의 디폴트는 true이다. 이는 **"내 child view들이 자신의 영역 안에서만 그리도록 clip 시키겠다"**라는 의미이다. 그래서 이 속성이 디폴트일때의 동작은 아래와 같다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/clipchildren_mov01.gif" alt="when clipChildren is true">
  <figcaption>clipChildren이 true일 때</figcaption>
</figure> 

위의 레이아웃은 아래와 같이 간단하다:

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

        <!-- 이미지 -->
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

노랑이안에 검정이가 있고, 검정이 안에 이미지가 있는 단순한 레이아웃이다. 이 상태에서 이미지를 위로 translate 시키는 애니메이션을 돌리면, 검정이가 이미지를 그리는 위치가 서서히 위로 올라간다. 그러다 검정이의 영역을 벗어나는 시점부터는 이미지가 그려지지 않는다. 노랑이의 clipChildren 값이 true라는 말은, 노랑이가 검정이에게 **"너의 영역만큼 canvas를 clip해서 그 안에서만 그리도록 해"**라고 명령했다는 뜻이기 때문이다.

자 그럼 노랑이의 clipChildren 값을 false로 바꿔보자:

```xml
<!-- 노랑이 -->
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="#F8E24C"
    android:clipChildren="false">

    ... 생략
```

그리고 다시 실행하면 아래와 같이 동작한다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/clipchildren_mov02.gif" alt="when clipChildren is false">
  <figcaption>clipChildren이 false일 때</figcaption>
</figure> 

검정이가 자신의 영역 밖으로도 이미지를 그렸다! 노랑이의 clipChildren 값이 false이기 때문에, 노랑이가 검정이에게 **"너의 사이즈만큼 clip하지 말고 그냥 내 canvas 그대로 써!"** 라고 했기 때문이다. 결국 검정이가 **'차지하는 영역'**은 변함없이 검정 색깔의 공간 만큼이지만, 검정이가 **'그릴 수 있는 영역'**은 노랑이의 canvas 그대로가 되는 것이다. 

여기서 배울 수 있는건 **view가 차지하는 영역과 그릴 수 있는 영역이 반드시 같지는 않다는 것이다**. 실제로는 안드로이드 프레임워크에서 이 속성값을 보고 손수 동일하게 잡아주고 있던 것인데, 디폴트 값 덕분에 우리가 당연시 해왔던 것이다. clipChildren을 이용해서 이 동작을 바꿀 수 있다.

주의할 점! 위에서 봤듯이 **노랑이 안에 검정이가 있고, 검정이가 자신의 영역 밖으로 그릴 수 있게 하려면, 검정이의 clipChildren 값이 아닌 노랑이의 값을 바꾸는 것이다**. 헷갈리지 말자!

---

번외로, 이 속성이 실제로 어떻게 적용되는 것인지 프레임워크 소스를 간단히 살펴보자. 위 예제를 다시 사용해보자. 우선, 노랑이가 자신의 child(검정이)를 그리기 위해 drawChild 함수를 부른다:

```java
// ViewGroup.java

protected boolean drawChild(Canvas canvas, View child, long drawingTime) {
  return child.draw(canvas, this, drawingTime);
}
```

그럼 검정이의 draw 함수가 불리는데, 구현부의 일부를 간소화하면 아래와 같다:

```java
// View.java

boolean draw(Canvas canvas, ViewGroup parent, long drawingTime) {
  // ... 생략

  // parent가 노랑이다. 노랑이의 flag 값을 가져온다.
  final int parentFlags = parent.mGroupFlags;

  // ... 생략

  // 노랑이의 clipChildren 값이 true면, 검정이가 스스로 자신의 영역만큼 canvas를 clip한다.
  if ((parentFlags & ViewGroup.FLAG_CLIP_CHILDREN) != 0) {
    canvas.clipRect(sx, sy, sx + getWidth(), sy + getHeight());
  }

  // ... 생략
}
```

즉, parent의 clipChildren값이 true면 ```canvas.clipRect```를 부르기 때문에 자신의 영역 밖으로는 그릴 수 없게 되고, 반대로 false면 ```canvas.clipRect```를 부르지 않기 때문에 parent가 가지고 있는 canvas를 그대로 온전히 이용하게 되는 것이다.
