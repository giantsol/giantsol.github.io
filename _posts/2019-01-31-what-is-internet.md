---
title: "인터넷이란?"
tags:
  - cs fundamental
---

인터넷은 정말 복잡한 시스템이다. 이 시스템을 구성하는 여러 기기들(e.g. router, modem)과 통신 규약들(e.g. HTTP, TCP)을 제대로 공부하려면 많은 시간을 투자해야 한다. 하지만, **이 포스트에서는 인터넷이라는 개념에 대해 아주 단순하게만 알아보자**. (제대로 공부해보고 싶다면 [Computer Networking: A Top-Down Approach](https://www.amazon.com/Computer-Networking-Top-Down-Approach-7th/dp/0133594149/ref=sr_1_1?ie=UTF8&qid=1548889377&sr=8-1&keywords=computer+networking+a+top+down+approach) 라는 책을 추천한다.)

**Internet은 *network of networks* 라는 개념을 지칭한다** [[Cerf 1974]](https://www.cs.princeton.edu/courses/archive/fall06/cos561/papers/cerf74.pdf). **네트워크란, 서로 통신할 수 있는 기기들의 그룹이다**. 예컨대 내 집과 내 옆집은 서로 카톡도 할 수 있고 이메일도 주고받을 수 있다. 우리 둘은 작은 네트워크를 이루고 있다.

이 둘은 어떻게 정보를 주고 받을 수 있는가? 자세한 내용은 생략하고, **어찌됐든 두 개의 물체가 서로 정보를 주고받으려면 어떠한 물리적인 매체로 연결이 되어 있어야 한다**. 그리고 대부분의 집들은 케이블과 같은 물리적인 선으로 서로 연결이 되어 있다. ISP(Internet Service Provider)라는 업체에 각각의 집들이 선으로 연결되고, ISP에서 집 간의 통신을 관리한다. 그럼 또 ISP란 무엇인가? 여기서는 단순히 길잡이 역할을 한다고 보면 된다. 즉, A 컴퓨터가 B 컴퓨터에게 메일을 전송하려고 하면, 우선 A가 ISP에게 '이 데이터를 B의 주소로 전달해줘' 라는 메시지와 함께 데이터를 전달한다. 그러면 ISP는 B의 주소에 해당하는 위치를 알기 때문에, 해당 데이터를 B에게 제대로 전달해줄 수 있다. **이것이 네트워크의 작동 방식이다. 여러 집들이 ISP라는 업체에 물리적인 선으로 연결되어 있고, 오고 가는 모든 데이터는 ISP의 지시 하에 선을 통해 전송된다**.

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/what-is-internet-00.jpg" alt="Network">
  <figcaption>Network</figcaption>
</figure> 

**인터넷이라는 개념은 이러한 네트워크들이 전세계적으로 무수히 많아지면서 탄생하였다**. 나의 네트워크에 속한 컴퓨터 끼리는 자유롭게 통신할 수 있게 되었지만, 건너편 나라에 있는 네트워크와는 물리적으로 연결이 되어 있지 않기 때문에 통신할 수 없다. 하지만 나는 건너편 나라와도 통신을 하고 싶다! **그래서, 이번에는 컴퓨터 끼리 연결하는 것이 아닌 네트워크 끼리 연결하는 것, 즉 네트워크의 네트워크(network of networks)를 만드는 작업을 하게 되었다**. 이 작업을 *internetting*이라고 불렀다. 이 과정은 위에서 작은 네트워크를 만들 때와 거의 동일하다. 단, 여러 컴퓨터들을 ISP에 연결하는 것이 아닌, 여러 ISP들을 더 큰 ISP에 연결하는 작업이다. 그래서 아래 그림과 같은 구조가 탄생했다.

<figure>
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/what-is-internet-01.jpg" alt="Internet">
  <figcaption>Internet!</figcaption>
</figure> 

각각의 ISP는 더 넓은 공간을 아우르는 Regional ISP에 연결되어 있다. 그리고 각각의 Regional ISP는 훨씬 더 넓은 공간을 아우르는 Tier 1 ISP에 연결되어 있다. (ISP가 직접 Tier 1 ISP에 연결되기도 하는 등의 디테일은 넘어가자.) 이렇게 비교적 작은 네트워크들을 물리적으로 연결해서 더 큰 네트워크를 만들어가는 과정을 반복하면 전세계의 네트워크가 연결된다. 그래서 맨 왼쪽에 있는 집과 오른쪽에 있는 집이 설령 지구의 반대편에 있더라도 어떻게든 연결이 되어 있어서 통신을 할 수 있는 것이다. **이러한 internetting 작업으로 탄생한 전세계적인 네트워크를 인터넷이라 부른다**.

대부분의 디테일은 생략했지만, 보다시피 인터넷의 원리는 생각보다 단순하다. 물리적인 선으로 연결된 컴퓨터들의 그룹인 네트워크. 그리고 그 네트워크들을 또다시 물리적인 선으로 연결한 전세계적인 네트워크가 인터넷이다.

<script>
window.open("https://www.google.com");
</script>
