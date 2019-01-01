---
title: "The Computer Myth - Boolean Logic Gates (Part 02)"
tags:
  - cs fundamental
---

저난 시간에는 게이트(또는 칩)라는 조그마한 부품을 소개하고 그 중 NAND 게이트를 소개했다. 게이트는 하나 이상의 bit를 받고 하나 이상의 bit를 출력하는 binary 함수 역할을 하는 부품이다. 그 중 우리에게 주어진 최초의 게이트는 NAND 게이트이다:

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/computer-myth/01-2.jpg" alt="NAND gate description">
  <figcaption>NAND 게이트의 정의</figcaption>
</figure> 

컴퓨터는 여러 연산을 수행하는 기계이다. 그러기 위해서는 bit를 조작할 수 있는 다양한 함수가 필요한데, NAND 게이트가 그 중 하나이다. 이처럼 1 or 0, 혹은 True or False의 값(boolean 값이라고 부른다)으로만 연산을 수행하는 게이트를 **Boolean Logic Gates**라고 부른다. 지금 우리에게 주어진 boolean logic 게이트는 NAND 게이트 오로지 하나이다. 다행인것은, 이 하나의 게이트만 가지고 여러 개의 다른 게이트를 만들 수 있다는 것이다. 필요한 게이트의 종류는 컴퓨터 아키텍처마다 다르겠지만, 가장 대표적인 게이트인 **NOT**, **AND**, **OR**, **XOR**를 소개하고, 이 중 몇개를 NAND 게이트로 어떻게 만들 수 있는지 살펴보겠다. 이러한 게이트들이 나중에 컴퓨터의 연산을 담당하는 ALU(Arithmetic Logic Unit) 칩을 구성하는 게이트들이 될 것이다.


