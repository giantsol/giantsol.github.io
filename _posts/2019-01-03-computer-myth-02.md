---
title: "The Computer Myth - Boolean Logic (Part 02)"
tags:
  - cs fundamental
---

게이트(또는 칩)는 하나 이상의 bit를 받고 하나 이상의 bit를 출력하는 함수 역할을 하는 조그마한 부품이다. 그 중 우리에게 주어진 최초의 게이트는 [NAND 게이트였다](/computer-myth-01/):

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/01-2.jpg" alt="NAND gate description">
  <figcaption>NAND 게이트의 정의</figcaption>
</figure> 

컴퓨터는 다양한 연산을 수행하는 기계이다. 근데 컴퓨터는 결국 0 또는 1의 bit밖에 이해하지 못하므로, 결국 이러한 bit들을 조작할 수 있는 다양한 함수가 필요하다는 뜻이다. bit가 가질 수 있는 값을 True(1) 또는 False(0)라고도 부르며, 이렇게 True/False 두 개로 나뉘는 값을 **boolean**이라고도 부른다. 그리고 이러한 boolean 값으로만 수행되는 연산들을 **Boolean Logic**이라고 부른다.

다시 말하지만, 게이트는 1 또는 0의 값을 받고, 1 또는 0의 값을 출력하는 boolean 값만 다루는 조그마한 부품이다. **그래서 게이트를 boolean logic gate, 혹은 logic gate라고도 부른다**. 컴퓨터는 결국 0 또는 1이라는 boolean값으로만 연산을 수행하기 때문에, 다양한 boolean logic을 수행하는 다양한 게이트를 필요로한다. NAND 게이트는 그 중 하나일 뿐이다.

그럼 그 다양한 게이트들은 다 어떻게 만들어야하나? 다행히, **모든 게이트들은 더 단순한 게이트들의 조합으로 만들어질 수 있다**. 예컨대, **NOT** 게이트는 아래와 같이 정의된다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-0.jpg" alt="NOT gate description">
  <figcaption>NOT 게이트의 정의</figcaption>
</figure> 

in으로 들어온 bit를 반전시키는 boolean logic을 수행하는 게이트다. 이 게이트는 아래와 같이 NAND 게이트 하나로 구현할 수 있다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-1.jpg" alt="NOT gate implementation">
  <figcaption>실제로 맞는지 in에 값을 대입해보며 추적해보자!</figcaption>
</figure> 

함수를 구현할 때 다양한 방법이 있듯이, 하나의 게이트를 만드는 방법도 다양하다. 위는 하나의 예시일 뿐이다. 아무튼 중요한건, **다른 더 복잡한 게이트들도 똑같은 원리로 구현이 가능하다는 점이다**. 이제는 NAND 뿐만이 아니라 NOT 게이트도 수중에 있으니 더 쉽다.

그 외 자주 등장하는 **OR**, **XOR**, 그리고 **AND** 게이트는 간단히 정의만 살펴보겠다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-2.jpg" alt="OR gate description">
  <figcaption>OR 게이트: a, b 중 하나라도 1이면 1을 출력한다</figcaption>
</figure> 

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-3.jpg" alt="XOR gate description">
  <figcaption>XOR 게이트: OR과 유사하나 둘 중 하나만 1일 때 1을 출력한다</figcaption>
</figure> 

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-4.jpg" alt="AND gate description">
  <figcaption>AND 게이트: 둘 다 1일때만 1을 출력한다(NAND와 반대)</figcaption>
</figure> 

이 외에 [MUX, DMUX](https://en.wikipedia.org/wiki/Multiplexer) 게이트도 자주 등장하는데, 이는 필요할 때 설명하겠다.

정리하자면, 
1. 컴퓨터는 오로지 0 또는 1이라는 bit만 이해하고, 이렇게 참/거짓으로 나뉘는 값을 **boolean**이라고 부른다.
2. Boolean 값으로만 이루어진 연산들을 **boolean logic**이라고 부르고, 게이트는 이러한 boolean logic을 수행하는 부품으로써 **boolean logic gate**라고도 불린다.
3. 컴퓨터는 bit를 다양하게 조작하기 위해 다양한 게이트를 지니는데, NAND, AND, OR, XOR가 그런 것들이다.
4. **모든 복잡한 게이트는 항상 더 단순한 게이트들의 조합으로 구현될 수 있다**.

(4)번이 여기서 배워갈 가장 중요한 키포인트다. **컴퓨터라는 거대한 시스템도 결국 하나의 거대한 게이트이다. 그리고 모든 게이트는 더 단순한 게이트의 조합으로 구현된다**. 우리가 만들어갈 컴퓨터 아키텍처는 현미경으로 들여다보면 수없이 많은 NAND 게이트로 이루어져 있을 것이다(다시 말하지만 반드시 NAND 게이트일 이유는 없다!). 우리가 곧 보게 될 **ALU 칩**은 컴퓨터의 모든 수학적, 논리적 연산을 담당하는 중요한 부품인데, 이 칩도 우리가 여기서 본 게이트들의 조합으로 만들어질 것이다. 

---

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/02-5.JPG" alt="Computer Myth pathway in part 02">
  <figcaption>지금까지의 여정!</figcaption>
</figure> 

NAND 게이트와 이를 기반으로 한 기본적인 게이트들까지 알아보았다. 이제 ALU 칩에 대해 알아볼 준비가 거의 다 되었다. 마지막으로 [Boolean Arithmetic](/computer-myth-03/)이라는 개념만 살펴보자.
