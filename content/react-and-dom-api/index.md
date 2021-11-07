---
emoji: ✏️
title: react 컴포넌트와 DOM api
date: '2021-10-08 00:00:00'
author: 장희성
tags: react
categories: 프론트엔드
---

![](https://images.velog.io/images/heesungj7/post/7e27d877-5adb-47ab-85e6-25ca0cb2719a/unnamed.jpeg)

다들 처음 프로그래밍 언어 또는 웹 개발을 공부를 시작하면서 그 유명한 **"hello, world"**를 터미널 또는 브라우저에 찍어본 경험이 있을꺼다.

DOM과 HTML을 어느정도는 알고있다면 "Hello, World"를 브라우저 화면에 쉽게 출력할 수 있을꺼라 생각한다. 또한 스크립트 태그 안에서 자바스크립트 코드를 통해 DOM 조작을하고 웹 페이지와 상호작용을 할 수 있다.

```javascript
<html>
  <body>
    <div>Hello World</div>
    <script type="module">// 자바스크립트 코드</script>
  </body>
</html>
```

다양한 웹 프레임워크가 나오기 이전에는 서버에서 HTML을 생성하고 브라우저와 상호작용을 하기 위해 HTML 위에 자바스크립트 코드를 추가했다.

하지만 이러한 방식은 웹 페이지가 복잡해지고 개발적으로 요구사항이 까다로워지면서 유지보수나 성능 측면에서 문제가 발생하기 시작했고 이를 해결하기 위해 우리가 알고있는 vue나 react같은 다양한 웹 프레임워크가 등장했다. 만약 지금 리액트를 사용하지 않고 서비스를 만들어여 한다면...🥲

그만큼 리액트가 좋다 😅

<hr/>

# 그래서 뭐 DOM이 왜?

리액트는 세계에서 가장 널리 사용되는 프런트엔드 프레임워크이고 이 블로그 시리즈 또한 결국 리액트를 조금 더 깊게 이해하고 싶은 마음에 시작했다.

### 🤔 그렇다면 DOM을 이해하는게 리액트를 더 잘 알기 위해서 왜 중요한가?

```Javascript
<html>
  <body id="root">
    <script type="module">
      const rootElement = document.createElement('div')
      document.body.append(rootElement)

      const element = document
        .createElement('div')
        .setAttribute('class', 'container')
        .textContent('Hello World')

      root.append(div)
    </script>
  </body>
</html>

```

위에서 사용했던 예시를 HTML 태그를 직접 작성하지 않고 자바스크립트를 사용하여 **"Hello World"라는 텍스트로 div DOM 노드를 만들고 해당 DOM 노드를 문서에 삽입하여** 자바스크립트 코드만으로 같은 출력 결과를 얻을 수 있다.

> 📌 세계에서 가장 널리 사용되고 프런트엔드 프레임워크인 리액트도 DOM 노드를 만들 때 사용하는 것과 동일한 API를 사용한다.

```javascript
if (type === 'select') {
        const node = ((domElement: any): HTMLSelectElement);
        if (props.multiple) {
          node.multiple = true;
        } else if (props.size) {
          // Setting a size greater than 1 causes a select to behave like `multiple=true`, where
          // it is possible that no option is selected.
          //
          // This is only necessary when a select in "single selection mode".
          node.size = props.size;
        }
      }
    }
  } else {
    domElement = ownerDocument.createElementNS(namespaceURI, type);
  }

  if (__DEV__) {
    if (namespaceURI === HTML_NAMESPACE) {
      if (
        !isCustomComponentTag &&
        Object.prototype.toString.call(domElement) ===
          '[object HTMLUnknownElement]' &&
        !Object.prototype.hasOwnProperty.call(warnedUnknownTags, type)
      ) {
        warnedUnknownTags[type] = true;
        console.error(
          'The tag <%s> is unrecognized in this browser. ' +
            'If you meant to render a React component, start its name with ' +
            'an uppercase letter.',
          type,
        );
      }
    }
  }

  return domElement;
}
```

리액트 소스코드의 일부분이다. 사실 이 코드가 어떤 동작을 하는지 알지는 못하지만 리액트가 흔히 말하는 어떠한 "매직"을 일으키는게 아니라 내부적으로는 우리가 알고있는 browser API를 사용해서 DOM 요소를 만들고 있다는게 중요하다.

- React 이전에는 DOM을 직접적으로 조작하는 방식이였고 이러한 방식을 **명령형(imperative) 프로그래밍** 이라고 한다.

- 이 방식의 문제점은 여러 이벤트 간의 연결을 파악하기 힘들다는 점이다.

- 또한 DOM을 직접적으로 조작하는 것은 performance에 큰 영향을 미친다.

즉, 리액트는 이러한 문제점을 보안해주기 위해 기존 명령형 방식을 내부적으로 처리해서 **선언형(declarative) 프로그래밍적**으로 API를 사용할 수 있게 해준다.

<hr/>

## React & ReactDOM

리액트에 익숙하다면 아래 구문을 잘 알고 있을꺼다. App.js 컴포넌트를 작업하고나 새로운 컴포넌트를 만들면서 무수히 많이 작성했을꺼다.

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
```

일단은 리액트의 문법인 JSX를 사용하지 않고 리액트가 제공하는 API를 통해서 직접적으로 위에서 만든 DOM 요소를 리액트 요소로 변경해보자.

> 기억해야 할게 리액트는 리액트 네이티브와 같이 브라주저가 아닌 다른 플랫폼 개발도 지원하기 때문에 React API 외에도 각 플랫폼에 맞춰진 별도의 API를 제공한다. 웹의 경우 ReactDOM을 사용한다.

- **React**: document.createElement()와 비슷한 React.createElement같은 메소드를 제공한다.
- **ReactDOM**: DOM에 요소를 랜더링할때 사용한다.

```javascript

<body>
  <div id="root"></div>

  <script src="https://unpkg.com/react@17.0.0/umd/react.development.js"></script>
  <script src="https://unpkg.com/react-dom@17.0.0/umd/react-dom.development.js"></script>

  <script type="module">
    const rootElement = document.getElementById('root')

    const helloElement = React.createElement('span', null, 'Hello')
    const worldElement = React.createElement('span', null, 'World')

    const reactElement = React.createElement('div', {
      className: 'container',
      children: [helloElement, ' ', worldElement],
    })

    ReactDOM.render(reactElement, rootElement)
  </script>
</body>
```

#### 🖥 브라우저 출력화면:

![](https://images.velog.io/images/heesungj7/post/05527512-46c7-4128-aec2-02a91e81f4aa/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-20%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.58.39.png)

```toc

```
