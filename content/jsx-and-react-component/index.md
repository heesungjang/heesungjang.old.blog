---
emoji: ✏️
title: jsx와 리액트 컴포넌트
date: '2021-10-08 00:00:00'
author: 장희성
tags: react
categories: 프론트엔드
---

[참고: Kent C Dodds - What is JSX?](https://kentcdodds.com/blog/what-is-jsx)

## 📚 JSX

> "This funny tag syntax is neither a string nor HTML" - 리액트 공식 문서

JSX는 Raw React API보다 직관적이며 코드를 읽을 때 이해하기 쉽게 만들어주는 **syntactic sugar이다**.

그치만 JSX은 그 자체로 자바스크립트는 아니기 때문에 브라우저가 이를 이해하기 위해서는 **babel**과 같은 코드 컴파일러가 필요하다.

실제 [바벨](https://babeljs.io/repl#?browsers=defaults%2C%20not%20ie%2011%2C%20not%20ie_mob%2011&build=&builtIns=App&corejs=3.6&spec=false&loose=false&code_lz=MYewdgzgLgBArgSxgXhgHgCYIG4D40QAOAhmLgBICmANtSGgPRGm7rNkDqIATtRo-3wMseAFBA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=false&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=react&prettier=true&targets=&version=7.15.7&externalPlugins=&assumptions=%7B%7D) 사이트에 들어가보면 내가 작성한 JSX 코드가 어떻게 컴파일되어 브라우저가 이해하는 형식으로 변한하는지 확인할 수 있다.

```javascript
const ui = <h1 id="greeting">Hey there</h1>;

// ↓ ↓ ↓ ↓ babel 컴파일후 ↓ ↓ ↓ ↓

const ui = React.createElement('h1', { id: 'greeting', children: 'Hey there' });
```

[리액트 알아보기[1]](https://velog.io/@heesungj7/%EB%A6%AC%EC%95%A1%ED%8A%B8-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B01-DOM-Javascript-and-React)에서 기존 DOM 생성을 React API를 사용해서 변경했던 코드를 JSX 문법으로 변환해보자.

```javascript
const rootElement = document.getElementById('root');

const helloElement = React.createElement('span', null, 'Hello');

const worldElement = React.createElement('span', null, 'World');

const reactElement = React.createElement('div', {
  className: 'container',
  children: [helloElement, ' ', worldElement],
});

// ↓ ↓ ↓ ↓ JSX 문법 ↓ ↓ ↓ ↓

const children = 'Hello World';

const element = <div className={className}>Hello World</div>;
```

<hr/>

## 📚 리액트 커스텀 컴포넌트

일반 자바스크립트에서 함수를 만들어 재사용 가능한 코드를 만드는 것과 같이 JSX또한 함수를 통해 재사용 가능한 JSX 코드를 작성할 수 있고 이를 리액트에서는 **Componenet**라고 부른다.

#### 📌 html을 작성해서 DOM에 아래와 같이 그리고 싶다고 해보자.

```html
<div class="container">
  <div class="message">Hello World</div>
  <div class="message">Goodbye World</div>
</div>
```

React.createElement의 첫 번째 인수로는 기존에 "div", "span"으로 전달하던 type 말고도 랜더링 가능한 리액트 요소를 반환하는 함수를 직접 전달할 수 있기 때문에, 아래와 같이 인수 값으로 하위 속성(children)을 받아 리액트 요소를 반환하는 함수를 만들면 코드 중복을 해결 할수 있다.

```javascript
const msg = (props) => {
  return <div className="container">{props.children}</div>;
};

const HelloElement = React.createElement(msg, {
  children: ['Hello World', 'Goodbye World'],
});

ReactDOM.render(HelloElement, document.getElementById('root'));

// ↓ ↓ ↓ ↓ babel 컴파일 ↓ ↓ ↓ ↓

var element = /*#__PURE__*/ React.createElement(
  'div',
  {
    className: 'container',
  },
  /*#__PURE__*/ React.createElement(
    'div',
    {
      className: 'message',
    },
    'Hello World',
  ),
  /*#__PURE__*/ React.createElement(
    'div',
    {
      className: 'message',
    },
    'Goodbye World',
  ),
);
```

#### 🔥 JSX 문법으로 변경해보자

```javascript
const msg = ({ children }) => {
  return <div className="Message">{children}</div>;
};

const element = (
  <div>
    <message>Hello World</message>
  </div>
);

ReactDOM.render(element, document.getElementById('root'));
```

```toc

```
