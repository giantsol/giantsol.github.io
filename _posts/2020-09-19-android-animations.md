---
title: "안드로이드의 애니메이션 api 정리"
tags:
  - android
---

안드로이드에서 애니메이션을 구현하기 위한 방법은 정말 다양하다.
너무 다양해서 뭘 써야할지 선택 장애가 올텐데, 안드로이드 팀도 그걸 알아서 그런지 쭉 비디오로 정리해 주었다.
아래 내용은 [이 비디오](https://www.youtube.com/watch?v=N_x7SV3I3P0)와 [이 비디오](https://www.youtube.com/watch?v=f3Lm8iOr4mE)를 참고하여 만들어졌으니, 비디오만 봐도 내용은 비슷하다.

애니메이션 api 종류:
1. ViewAnimations (API 1)
2. ValueAnimator, ObjectAnimator (API 11)
3. ViewPropertyAnimator (API 12)
4. Transitions (API 19)
5. AnimatedVectorDrawable (API 21에 추가되었지만 AndroidX로 하위 버전도 사용 가능)
6. Physics, MotionLayout (AndroidX)

각각의 특징을 간략하게 알아보자.

### ViewAnimations

- **안드로이드 가장 초기에 쓰던 방식**으로, res/anim/xxx.xml에 정의해서 사용했다.
- 다른 위치에 그리기만 할 뿐, 실제로 그 위치로 이동되는 것은 아니기 때문에, 이동된 위치에서의 클릭은 먹지 않는다.
- **사실상 deprecated된 기능**으로, 윈도우 애니메이션 정도에서만 쓰는게 이상적이다.

### ValueAnimator, ObjectAnimator

- ValueAnimator는 그 이후에 생긴 다른 애니메이션 api의 기반이 될 정도로 **훨씬 강력해진 api**이다 (ObjectAnimator도 ValueAnimator를 기반으로 동작한다).
- ValueAnimator는 코드에서 `ValueAnimator.ofFloat(0f).addUpdateListener { ... }.start()` 식으로 사용한다.
- ObjectAnimator는 코드에서 `ObjectAnimator.ofFloat(view, View.ALPHA, 0f).start()` 식으로 할 수도 있고, res/animator/xxx.xml에서 `<objectAnimator>` 태그로도 사용될 수 있다.
- ObjectAnimator를 사용할 때는 `ObjectAnimator.ofFloat(view, "alpha", 0f)`처럼 reflection으로 사용하기 보다는 위와 같이 `View.APHA`로 사용하는게 더 좋다.

### ViewPropertyAnimator

- `view.animate().alpha(0f).start()` 식으로 사용되는 api로, 이 또한 ValueAnimator를 기반으로 동작한다.
- 애니메이션이 진행 중에 다시 `start()`를 하면, 이전 애니메이션이 멈춘 지점부터 다시 시작한다.
- 밖에서 따로 애니메이션의 진행 상황을 관리할 수는 없기 때문에, **애니메이션을 실행시키고 신경쓸 필요 없는 fire-and-forget 형식**으로 쓰인다.

### Transitions

- 단순히 **여러 뷰들에 대한 ValueAnimator들을 한번에 정의할 수 있는 방법**이라고 보면 된다.
- **Shared Element Transition**을 구현하기 가장 쉬운 방법이다.
- xml에 커스텀 transition을 정의하여 사용할 수도 있고, 또다른 간단한 사용법으로는 `TransitionManager.beginDelayedTransition(viewGroup)`을 호출한 다음 해당 viewGroup에 있는 뷰들의 속성을 바꾸면,
다음 프레임에서 이전 뷰들의 속성과 바뀐 뷰들의 속성의 diff를 계산하여 animator들을 알아서 생성하여 실행시킨다.
- 사실 Shared Element Transition 외에는 별로 사용되지는 않는다.

### AnimatedVectorDrawable

- 움직이는 벡터 아이콘을 만들기 위한 api로, 아주 간단한 `start()`, `stop()` 등만 지원한다.
- AndroidX 사용시 `AnimatedVectorDrawableCompat`으로 사용하게 된다.
- res/drawable/xxx.xml 파일에 `<animated-vector>` 태그를 통해 사용할 수 있는데, minSdk가 21 이하인 경우 빨간줄이 뜨긴 하겠지만, androidX를 사용하면 문제 없이 돌아간다.
- 특정 지점으로 seek 하는 등읜 기능은 지원하지 않기 때문에 **fire-and-forget 형식**으로 사용되며, 더 섬세한 컨트롤이 필요한 경우 [Lottie](https://github.com/airbnb/lottie-android)를 사용하도록 한다.

### Physics

- 위 모든 애니메이션들은 정의된 interpolator에 따라 동작한다면, physics 라이브러리에 포함된 `SpringAnimation`과 `FlingAnimation`은 물리학에 따라 움직이기 때문에, 더욱 **"실제" 같은 움직임을 만들 때** 쓰인다.
- 아직 많은 주목을 받지 못하고 있는데, **사실 아주 범용적으로 잘 쓰일 수 있는 api**이다. 특히 [이 부분](https://youtu.be/f3Lm8iOr4mE?t=700)부터 보면 `SpringAnimation`을 활용해 거의 모든 자연스러운 움직임을 구현할 수 있음을 볼 수 있다.

### MotionLayout

- ConstraintLayout 2.0.0 부터 추가된 api로, ConstraintLayout과 TransitionManager의 기능을 짬뽕한 듯한 친구이다.
- ConstraintLayout의 다양한 레이아웃 기능들과, 자동으로 애니메이션을 생성해주는 Transition이 만나 xml로 아주 다양한 애니메이션 효과를 만들어낼 수 있는 api이다.
- KeyFrame이라는 기능도 있는데, 이 기능은 MotionLayout에서 처음 나온 기능이다.
- 아직 나온지 얼마 안되어서 샘플이 많지는 않지만, 가능성은 정말 무궁무진하다.

쭉 간단히 특징들을 살펴보았는데, 그럼 가장 중요한 "**언제 뭘 쓰면 되는데?**"를 정리해보자.
아래는 애니메이션 api를 고를 때 생각의 흐름이라고 보면 된다:

1. 아주 컴플렉스한 애니메이션들을 갖춘 화면을 만들고 싶다? => **MotionLayout**
2. 물리 법칙에 의거한 애니메이션을 만들고 싶다? => **Physics**
3. Shared Element Transition을 구현하고 싶다? => **Transitions**
4. 움직이는 벡터 아이콘을 만들고 싶다? => **AnimatedVectorDrawable** (or **Lottie**)
5. 윈도우에 애니메이션을 적용하고 싶다? => **ViewAnimations**
6. 특별히 관리할 필요 없는 아주 간단한 뷰 애니메이션을 만들고 싶다? => **ViewPropertyAnimator**
7. 완전 커스텀한 애니메이션을 만들고 싶다? => **ValueAnimator**
8. 그 외 모든 뷰의 속성을 이용하는 애니메이션을 만들고 싶다? => **ObjectAnimator**