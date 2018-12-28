---
title: "32bit vs 64bit processors"
tags:
  - cs fundamental
---

요즘 나오는 프로세서는 32bit와 64bit 두 개로 나뉘어진다. 사실 이제는 64bit만 나온다고 볼 수 있다. **xxbit 프로세서라는 것은, 컴퓨터가 한번에 xxbit 길이의 명령어를 수행한다는 뜻이다**. 32bit 프로세서는 한번에 32bit 길이의 명령어를, 64bit는 64bit 길이의 명령어를 수행한다.

이는 즉, 64bit가 한번에 더 많은 명령어를 처리하기 때문에 **더 빠르고**, 무엇보다도 **더 많은 메모리를 사용할 수 있다**는 것이다. 32bit는 최대 32bit 길이의 명령어만 처리할 수 있기 때문에, 표현할 수 있는 메모리 주소도 2의32승, 약 4,300,000,000개의 주소이고, 이를 용량으로 치환하면 약 4.3GB의 메모리가 된다. 요즘 나오는 컴퓨터는 적어도 8GB의 RAM을 가지는데, 32bit 프로세서를 사용한다면 RAM의 절반밖에 이용할 수 없다는 뜻이다. 반면 64bit 프로세서는 2의64승까지의 메모리 주소를 표현할 수 있기 때문에, 요즘 나오는 RAM을 충분히 다 활용하고도 남는다.

**32bit 프로세서에서는 64bit 용으로 제작된 프로그램을 실행시킬 수 없다**. 왜냐하면 64bit 프로그램에서 내리는 명령어들은 64bit씩 처리해야 의미를 가지는데, 이를 32bit씩 끊어서 처리할 수는 없기 때문이다. **반면 64bit 프로세서에서 32bit 프로그램은 실행시킬 수 있다**. 맥은 잘 모르겠으나, 윈도우에서는 [WOW64](https://docs.microsoft.com/en-us/windows/desktop/winprog64/running-32-bit-applications)라는 내장 emulator 덕분이라고 한다. 64bit의 맥과 윈도우 둘 다 아직까지는 32bit 프로그램에 대한 호환성을 유지해주지만, 요즘의 컴퓨터 대부분이 64bit 프로세서를 사용하는 만큼 기존 32bit 프로그램들도 업데이트 하길 권장하고 있다([참고](https://support.apple.com/en-us/HT208436)).

윈도우에서 Program Files와 Program Files (x86) 폴더가 따로있는 이유가 64bit 프로세서에서 32bit, 64bit 프로그램을 동시에 지원하기 위해서이다. Program Files 폴더에는 64bit, Program Files (x86)에는 32bit 프로그램이 들어간다. *.dll*과 같은 확장자를 지닌 shared library는 32bit, 64bit용 따로 제작되는데, 프로그램 구동 시 어느 버전을 사용할건지를 폴더로 구분한 것이다. 만약 32bit 프로그램을 Program Files 폴더에 둔다? 흠.. 해보진 않았지만.. 이론적으로는 안돌아갈 것이다!

*PS)* 32bit 프로세서를 x86이라고도 부르고, 64bit 프로세서는 x64라고도 부르는데 이건 대체 뭘까? 왜 32bit를 더 높은 숫자인 x86이라고도 부르는거지? 이는 단순히 옛날부터 이어져온 네이밍 컨벤션이다([참고](https://stackoverflow.com/questions/29974425/why-is-windows-32-bit-called-windows-x86-and-not-windows-x32)). 64bit도 원래는 x86-64 라고 불렸지만 x64라고 줄여서 부르게 되었다고 한다.

참고:
- [https://www.howtogeek.com/56701/htg-explains-whats-the-difference-between-32-bit-and-64-bit-windows-7/](https://www.howtogeek.com/56701/htg-explains-whats-the-difference-between-32-bit-and-64-bit-windows-7/)
- [https://www.quora.com/What-is-the-meaning-of-64-bit-and-32-bit](https://www.quora.com/What-is-the-meaning-of-64-bit-and-32-bit)
- [https://support.apple.com/en-us/HT208436](https://support.apple.com/en-us/HT208436)
