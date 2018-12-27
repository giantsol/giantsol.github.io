---
title: "32bit vs 64bit processors"
tags:
  - cs fundamental
---

윈도우에서 Program Files와 Program Files (x86) 폴더가 따로 있는 이유가 뭘까? Program Files 폴더에는 64비트 프로그램들이 들어가고, Program Files (x86) 폴더에는 32비트 프로그램들이 들어간다.

요즘 프로세서는 32bit, 64bit 두 개로 나뉘어진다. 사실 이제는 64bit만 나온다고 볼 수 있다. *xx*bit 프로세서라는 것은, 컴퓨터가 한번에 *xx*bit 길이의 명령어를 수행할 수 있다는 뜻이다. 즉, 32bit 프로세서는 한번에 32bit 길이의 명령어를, 64bit는 64bit 길이의 명령어를 수행한다. 이게 무슨 차이가 있냐면, 64bit는 32bit 프로세서에 비해 한번에 더 많은 명령어를 처리하기 때문에 **빠르고**, 무엇보다도 **더 많은 메모리를 사용할 수 있다**. 예컨대, 4bit 프로세서가 있다면 한번에 0000에서 1111 사이의 명령어만 수행할 수 있고, 이는 즉 0 ~ 16 사이의 메모리 주소, 약 16byte의 RAM만 사용할 수 있다는 것이다.

때문에, 32bit 프로세서에서는 64bit 프로그램을 실행시킬 수 없다. 왜냐하면, 프로그램에서는 64bit 단위로 명령어들을 내리겠지만, 프로세서는 고작 32bit씩만 이해할 수 있기 때문이다.
반면, 64bit 프로세서에서는 32bit 프로그램도 실행시킬 수 있다. 맥은 잘 모르겠으나, 윈도우에서는 이게 가능한 이유가 **WOW64**라는 내장된 emulator가 있기 때문이라고 한다 ([링크](https://docs.microsoft.com/en-us/windows/desktop/winprog64/running-32-bit-applications)).

맥이나 윈도우 둘 다 아직까지는 32bit 프로그램에 대한 호환성을 유지해주지만, 요즘의 컴퓨터들 대부분이 64bit CPU를 사용하는 만큼 기존 32bit 프로그램들도 업데이트 하길 권장하고 있다 ([링크](https://support.apple.com/en-us/HT208436)). 64bit CPU가 보편화되어가고 있는 이유는 32bit에 비해 훨씬 더 많은 메모리를 사용할 수 있고, 연산속도도 더 빠르기 때문이다. 32bit 프로세서에서는 표현가능한 메모리 주소가 2의32승, 즉 4GB까지 밖에 안되기 때문에 메모리가 4GB를 넘어가면 그 여유분은 버리게 되는 꼴이다.
요즘 나오는 컴퓨터들은 메모리가 보통 8GB가 넘기 때문에, 32bit 프로세서로는 이 메모리들을 다 사용할 수 없다는 뜻이다.
반면, 64bit는 2의64승, 대략 1700만TB만큼의 메모리를 사용할 수 있기 때문에, 요즘 CPU들은 대부분 64bit로 나오는 것이다.

윈도우에서 **Program Files**와 **Program Files (x86)** 폴더가 따로있는 이유도 64비트 CPU에서 34비트, 64비트 프로그램을 동시에 지원하기 위해서이다.
32비트와 64비트 프로그램은 서로 다른 환경에서 동작해야 하는데, 이를 구분하기 위한 단순란 방식으로 폴더를 따로 둔 것이다.
예컨대, 프로그램들이 공용으로 사용하는 .dll 파일은 32비트와 64비트용이 따로 존재하는데, 어떤 것을 선택할지를 저 폴더를 기준으로 둔다.

출처:
- https://www.howtogeek.com/56701/htg-explains-whats-the-difference-between-32-bit-and-64-bit-windows-7/
- https://www.quora.com/What-is-the-meaning-of-64-bit-and-32-bit
- https://docs.microsoft.com/en-us/windows/desktop/winprog64/running-32-bit-applications
- https://support.apple.com/en-us/HT208436
