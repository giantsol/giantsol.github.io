---
title: "Where to put script tag in HTML?"
tags:
  - web
---

\<script\>태그를 마주친 순간 브라우저는 DOM의 처리를 멈추고 \<script\>의 다운로드 및 실행을 진행한다. 그리고 이 \<script\>의 실행이 끝나야 비로소 다시 DOM 프로세싱을 진행한다. 이는 \<script\> 안에서 어떤 짓을 할지 모르기 때문에 꼭 필요한 메커니즘이고, inline script이든 external script이든 상관없다.

```html
<head>
  ...
  <script type="text/javascript" src="file1.js"></script>
  <script type="text/javascript" src="file2.js"></script>
  <link rel="stylesheet" type="text/css" href="styles.css">
  <link rel="icon" type="image/png" href="/favicon.png">
  ...
</head>
```

위와 같이 \<script\>가 \<head\>안에 있는건 최악의 수다. *file1.js*가 다운로드되고 실행이 끝나야 *file2.js*를 진행하고, *file2.js*가 다운로드되고 실행이 끝나야 다음으로 넘어가게 된다. 즉, \<head\>안에 \<script\>가 많을수록 그 뒤의 렌더링이 지연되기 때문에 사용자에게 흰 화면만 보여지는 시간이 길어진다.

물론, 요즘의 브라우저는 \<script\>의 병렬 다운로드를 지원한다. 즉, 위 상황에서 *file1.js*, *file2.js*, *file3.js*를 동시에 다운로드한다. 하지만, 화면의 렌더링이 시작되기 위해서는 여전히 이 모든 \<script\>의 실행이 끝날때까지 기다려야 하기 때문에 느린건 마찬가지이다.

**따라서, \<script\> 태그를 \<head\>에 두는 것은 바람직하지 않다. 페이지가 사용자에게 다 렌더링 된 후인 \<body\>의 끝부분에 두는 것이 바람직하다.** 그리고 **여러 개의 작은 \<script\>들을 따로 두는 것 보다, 하나의 큰 \<script\>로 합치는게 낫다**. 25KB의 js 파일 4개를 따로 다운받기 보다는, 이들을 합쳐서 100KB 하나의 js 파일을 다운받는게 낫다는 것이다(각 요청마다 부가적인 request-response cycle이 추가되기 때문).

한편, css, image와 같은 다른 asset의 다운로드는 DOM 파싱을 방해하지 않고 진행되기 때문에 \<head\>안에 두는 것이 일반적이다.

---

### 번외: Dynamic Script Element 패턴

옛날 HTML에서는 이런 류의 패턴을 볼 수 있다:

```html
<body>
  ...
  <script>
    var script = document.createElement("script");
    script.type = "text/javascript";
    script.src = "file1.js";
    (document.head || document.body).appendChild(script);
  </script>
</body>
```

이러한 패턴은 요즘은 찾아보기 힘든 패턴이다. 여기서부터는 나만의 가정이지만, 이렇게 dynamic하게 script element를 만들었던 이유는 여러 script간의 실행 순서를 보장해야 할 때가 아닌가 싶다. 예컨대 *file1.js*라는 파일이 다 불려져야만 *file2.js*라는 파일이 실행 가능하다면, *file1.js*의 로딩이 끝난 시점에 *file2.js*를 실행하도록 할 수 있는 메커니즘이 필요했을 것이다. 위처럼 script element를 만들면, *onload()*와 같은 콜백을 달 수 있어서, script간의 실행 순서를 보장할 수 있게 된다. 그래서 아래와 같은 패턴이 자주 사용되었다:

```html
<body>
  ...
  ...
  <script type="text/javascript">
    // helper func for creating dynamic script element
    function loadScript(url, callback) {
      var script = document.createElement("script")
      script.type = "text/javascript";
      script.src = url;

      // script 로딩이 다 된 시점에서 해야 할 callback이 있을 때, 그 callback을 불러주는 시점
      if (script.readyState){  // IE
        script.onreadystatechange = function(){
          if (script.readyState == "loaded" || script.readyState == "complete"){
            script.onreadystatechange = null;
            callback();
          }
        };
      } else {  // Others
        script.onload = function(){
          callback();
        };
      }

      (document.head || document.body).appendChild(script);
    }
    
    // file1.js 가 다 준비되면 file2.js를 로드한다.
    loadScript("file1.js", function() { loadScript("file2.js") });
  </script>
</body>
```

하지만 Vue나 React같은 요즘의 웹 프레임워크에서는 알아서 여러 js 파일들을 한데 묶어 하나의 js 파일로 만들어주고 \<body\>의 끝부분에 삽입해준다. script간의 실행 순서를 HTML단에서 보장해줄 필요가 없어졌기 때문에 요즘은 저런 패턴이 안보이는 듯 하다.

참고:
- [https://www.oreilly.com/library/view/high-performance-javascript/9781449382308/ch01.html](https://www.oreilly.com/library/view/high-performance-javascript/9781449382308/ch01.html)
- [https://aria-grande.github.io/](https://aria-grande.github.io/)
