---
title: "Emulator vs Simulator"
tags:
  - cs fundamental
---

Emulator와 simulator는 둘 다 테스트용 툴로써 사용된다는 점에서 동일하다. 평소엔 별 생각없이 혼용해서 쓸 만큼 유사한 개념인데, 구분할 때는 확실히 구분해서 사용한다. 예컨대, 안드로이드 가상 머신은 emulator라고 부르고, 아이폰 가상 머신은 simulator라고 부른다.

emulator는 **실제 기기의 모든 부분을 그대로 구현한 가상 머신**이다. 안드로이드 emulator를 예로 들면, 안드로이드 폰의 CPU를 이루는 AND, NOR 게이트 등 부터 해서 0110.. 등의 바이너리 코드를 처리하는 방식 등을 Java와 같은 high-level language로 실제와 똑같이 모방한다. 안드로이드 실제 기기를 소프트웨어로 구현한 것이라고 볼 수 있다. 그래서 안드로이드 기기와 emulator는 서로 대체 가능하다. Emulator에서 프로그램이 실행되는 과정과 실제 기기에서 실행되는 과정이 완전히 동일하기 때문에, emulator에서 작동하면 실제 기기에서도 똑같이 작동할 것이라고 보장할 수 있다.

반면 simulator는 **실제 기기의 output만 모방할 뿐, 그 안에서 일어나는 모든 과정을 다 구현하지는 않는다**. 아이폰 simulator를 예로 들면, 아이폰의 CPU와 OS가 그대로 다 구현된 것은 아니지만, simulator 안에서 프로그램을 실행시키면, 실제 기기와 아주 유사한(또는 동일한) 결과물만 보여주기 위해 겉핥기식으로만 구현이 된 것이다. 따라서 emulator와는 다르게, simulator와 실제 기기간의 동작에는 차이가 있을 수 있으며(비록 똑같이 동작하도록 만드는게 목표지만), 서로 완전히 대체 가능하진 않다.

이처럼 emulator와 simulator의 내부 구현 방식이 다르기 때문에, 속도에서도 차이가 난다. 보통은 emulator가 훨씬 느리다. 왜냐하면 실제 기기에서 하드웨어 레벨로 처리될 바이너리 코드들을 emulator에서는 Java와 같은 high-level language로 돌려야하기 때문이다. 그래서 옛날 안드로이드 에뮬레이터는 부팅하는데에만 한참을 기다려야했다. 하지만 [HAXM](https://software.intel.com/en-us/articles/intel-hardware-accelerated-execution-manager-intel-haxm)(Hardware Accelerated eXecution Manager)같은 기술이 나오면서 emulator의 속도도 엄청 빨라졌다. 간단히 요약하자면 소프트웨어 상에서 돌리던 바이너리 계산들을 하드웨어에서 돌릴 수 있도록 해주는 것이다.

뭐, 평소에 이 둘을 구분할 일은 없지만, 이거 하나만 기억하면 될듯 하다:
> If a flight-simulator could really transport you from A to B, then it would be a flight-emulator.

출처:
- https://stackoverflow.com/questions/1584617/simulator-or-emulator-what-is-the-difference
- https://stackoverflow.com/questions/4544588/difference-between-iphone-simulator-and-android-emulator
