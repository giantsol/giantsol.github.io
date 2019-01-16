---
title: "The Computer Myth - ALU (Part 04)"
tags:
  - cs fundamental
---

ALU(Arithmetic Logic Unit)은 컴퓨터에서 모든 연산을 담당하는 아주 중요한 칩이다. 우리가 만들어갈 컴퓨터는 16bit 체계이므로, ALU 칩은 16bit 단위로 명령어들을 받게 될 것이다.

따라서 우선은 이전에 본 NAND, AND 게이트들도 16bit 단위로 동작하도록 만들어야 한다. 이른바 NAND16, AND16 게이트다. 이들의 구현은 간단하게도 게이트 16개를 나란히 두면 된다.

\<diagrams\>

수학적 연산을 수행하기 위한 Adder 게이트도 만든다. 이 게이트는 두 개의 bit 나열을 아래와같이 더하는 기능을 한다.

\<diagram\>
