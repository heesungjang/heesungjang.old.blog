---
emoji: ✏️
title: 리액트 key props 자세히 알아보기
date: '2021-09-23 00:00:00'
author: 장희성
tags: react
categories: 프론트엔드
---

![](https://images.velog.io/images/heesungj7/post/dfbc6ee9-8e5e-4b85-8056-abc53c6aef83/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-21%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2012.53.31.png)

리액트 개발을 해봤다면 배열을 렌더링 할때 위와 같은 ❌ 경고 메세지를 한번쯤 봤을거다. 이번 시간에는 리액트 Element에 왜 key props가 필요하고 어떤 점을 주의해야 하는지 알아보자.

## 🤔 조정(Reconciliation)

리액트 문서를 먼저 살펴보자:

> Key는 React가 어떤 항목을 변경, 추가 또는 삭제할지 식별하는 것을 돕습니다. key는 엘리먼트에 안정적인 고유성을 부여하기 위해 배열 내부의 엘리먼트에 지정해야 합니다.

리액트 공식 가이드에 따르면 key props가 컴포넌트의 변화가 생겼을때 어떤 항복을 변경, 추가, 삭제할지 돕는다고 한다.

컴포넌트의 상태 값이 변경되면 렌더링 함수는 이전 요소와 현재 요소의 트리를 반환한다.

React는 가장 최근의 변경 사항으로 UI를 효율적으로 업데이트하기 위해 어떤 차이가 있는지 파악해야 하고, 이 두 트리를 일치시키는 프로세스를 **조정(Reconciliation)**이라고 한다.

이때 변경 사항만 기존 rendering에 적용 시키기 위해서는 변경 전 / 후의 element tree를 비교가 필요하고 **이 tree 비교에 React는 2가지 가정을 추가해서 O(n)의 휴리스틱 알고리즘을 사용한다**.

<hr/>

## 그래서 key props가 왜 필요하다고?

위에서 언급했듯이 state이나 props가 갱신되면 리액트 render() 함수는 새로운 React 엘리먼트 트리를 반환단다. 이때 가장 **효과적으로 UI를 갱신하는 방법**이 필요한데 기존 알고리즘으로 이를 해결하려고 하면 성능 문제가 발생한다.

기존에 어떤 알고리즘을 선택했는지 공식 문서에서 명시하고 있지는 않지만 기존에 최첨단 알고리즘을 사용해도 하나의 트리를 가지고 다른 트리로 변화하기 위한 **최소한의 연산 수가 O(n3)의 복잡도**를 가진다고 한다.

리액트 팀의 공식 문서에 따르면 이러한 알고리즘을 리액트에 적용하면 1000개의 엘리먼트를 그리기 위해 10억 번의 비교 연산이 필요하다고 한다. 10억..😱 보기만해도 어플리케이션의 크기가 커지게 되면 UI를 그리기 위해 너무나도 비싼 연산이 필요하다.

## 휴리스틱 알고리즘

리액트는 대신, 두가지 가정을 기반하여 기존 O(n3)의 복잡도를 O(n) 복잡도의 **휴리스틱 알고리즘**을 구현했다고 한다.

#### ✅ 두 가지 가정

1. 서로 다른 타입의 두 엘리먼트는 서로 다른 트리를 만들어낸다.
2. 개발자가 **key prop**을 통해, 여러 렌더링 사이에서 어떤 자식 엘리먼트가 변경되지 않아야 할지 표시해 줄 수 있다.

📌 위 두가지 가정은 휴리스틱 알고리즘이 작동하기 위한 중요한 가정이고 이를 충족하지 못하게되면 왜 성능적인 불이익이 발생하는지 살펴보자.

### 엘리먼트의 타입이 다른 경우

[리액트 공식 문서 참고](https://ko.reactjs.org/docs/reconciliation.html#gatsby-focus-wrapper)
두 루트 엘리먼트의 타입이 다르면, React는 이전 트리를 버리고 완전히 새로운 트리를 반환한다. `<a>`에서 `<img>`로, `<Article>`에서 `<Comment>`로, 혹은 `<Button>`에서 `<div>`로 바뀌는 것 모두 트리 전체를 재구축하는 경우이다.

```JSX
<div>
  <Counter />
</div>

------⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️------- 변경

<span>
  <Counter />
</span>
```

위와 같은 경우는 DOM을 완전히 새로 그리는 경우로 이전 Counter 인스턴스는 파괴되고 새로운 Counter 인스턴스가 생성된다.

### 엘리먼트의 타입이 같은 경우

```JSX

  <div style={{color: 'red', fontWeight: 'bold'}} />

------⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️------- 변경

  <div style={{color: 'green', fontWeight: 'bold'}} />
```

이번 경우에는 div 타입이 span 타입으로 변경되었던 첫번째 경우와 다르게 동일한 타입을 가지기 때문에 엘리먼트는 동일한 속성은 유지하고 변경된 `color` 속성만 갱신한다.

이제 아래 실제 예시를 만들어 왜 우리가 반복되는 엘리먼트를 추가 할때 key prop을 넣어야 하는지 알아보자.

### 자식에 대한 재귀적 처리

DOM 노드의 자식들을 재귀적으로 처리할 때, React는 기본적으로 동시에 두 리스트를 순회하고 차이점이 있으면 변경을 생성한다.

```javascript
const items = [
  {id: 'apple', value: '🍎 apple'},
  {id: 'orange', value: '🍊 orange'},
  {id: 'grape', value: '🍇 grape'},
  {id: 'pear', value: '🍐 pear'},
]

------⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️------- 변경

const items = [
  {id: 'apple', value: '🍎 apple'},
  {id: 'orange', value: '🍊 orange'},
  {id: 'grape', value: '🍇 grape'},
]

```

자식을 재귀적으로 선회하면서 두개의 트리 즉 이전 트리와 현재 변경된 새로운 트리를 비교하는데 이때 위 예시와 같이 마지막 `🍐pear` 엘리먼트 즉, 자식의 끝에있는 엘리먼트가 추가되거나 삭제되면 두 트리의 사이의 변경은 잘 성능 문제없이 잘 작동한다.

```javascript
const items = [
  { id: 'pear', value: '🍐 pear' },
  { id: 'apple', value: '🍎 apple' },
  { id: 'orange', value: '🍊 orange' },
  { id: 'grape', value: '🍇 grape' },
];
```

이번에는 리스트의 맨 앞에 엘리먼트를 추가했다. 이 경우 리액트는 모든 요소가 제자리에 위치하지 않았다고 생각하고 종속트리는 유지 하지만 **모든 자식 엘리먼트를 새로 그리게되된다.** 이럴 경우에 리액트 공식 문서에 따르면 성능이 좋지 못하고 두 트리의 변환은 형편없이 작동한다고 한다.

### 리액트도 헷갈려...😢

이번 예시는 key props가 없는 상태에서 계속해서 추가나 삭제같은 변화가 일어나면 어떤 대재앙이 일어나는지 살펴보자.

```javascript
const element = {
  type: 'ul',
  key: null,
  props: {
    children: [
      {type: 'li', key: null, props: {children: '🍎 apple'}},
      {type: 'li', key: null, props: {children: '🍊 orange'}},
      {type: 'li', key: null, props: {children: '🍐 pear'}},
    ],
  },
}

------⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️------- 변경


const element = {
  type: 'ul',
  key: null,
  props: {
    children: [
      {type: 'li', key: null, props: {children: '🍊 apple'}},
      {type: 'li', key: null, props: {children: '🍐 pear'}},
    ],
  },
}

재조정(Reconciliation)

-       {type: 'li', key: null, props: {children: '🍎 apple'}},
-       {type: 'li', key: null, props: {children: '🍊 orange'}},
+       {type: 'li', key: null, props: {children: '🍊 apple'}},
```

이럴 경우에 결론만 본다면 '🍊 apple'과 '🍐 pear'이 생겼기 때문에 잘 작동했다고 할 수 있다. 하지만 조금 더 생각해보면 방금 일어난 재조정 연산은 순전히 **리액트의 예측**으로 이뤄진 결과이다.

### 리액트가 예측을 했다고??

자 위 예시에서

1. 🍎 apple이 🍊 apple 변경되었고
2. 🍊 orange가 삭제되고
3. 새로운 🍐 pear이 추가되었다.

이때 key prop과 같이 특정 요소를 지정할 수 있는 값이 없다면 리액트는 가장 괜찮은 예측으로 연산을 하는것이지 내가(개발자)가 어떻게 처리하려고 했는지 알 수가 없다.

리액트 입장에서 생각해보자:
리액트는 다음과 같은 고민을 하게된다.

1. 🍎 사과와 🍊 오렌지 리를 제거하고 🍊 사과와 함께 새 사과를 만들어야 하나?
2. 🍎 사과를 제거하고 🍊 오렌지를 🍊 사과로 이름을 바꿔야 하나?
3. 🍊 오렌지를 제거하고 🍎 사과 이름을 🍊 사과로 바꿔야하나?
4. 🍐 배를 제거한 다음 🍊 오렌지를 🍐 배로 이름을 바꾼 다음 🍎 사과를 🍊 사과로 이름을 바꿔야.......허..??

![source](https://user-images.githubusercontent.com/68782077/134453029-c3dfa7cc-f661-4289-a6b5-72381b8ad8a3.gif)

## 도와줘 key prop 🙏

```javascript
const element = {
  type: 'ul',
  key: null,
  props: {
    children: [
      {type: 'li', key: 'apple', props: {children: '🍎 apple'}},
      {type: 'li', key: 'orange', props: {children: '🍊 orange'}},
      {type: 'li', key: 'pear', props: {children: '🍐 pear'}},
    ],
  },
}

------⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️⬇️------- 변경

const element = {
  type: 'ul',
  key: null,
  props: {
    children: [
      {type: 'li', key: 'apple', props: {children: '🍊 apple'}},
      {type: 'li', key: 'pear', props: {children: '🍐 pear'}},
    ],
  },
}
```

자 이제 이렇게 엘리먼트 요소의 고유 key prop을 전달해주면 더이상 예측이 아닌 **정교하고 효율적인 연산이 가능해진다**.

예를들어 orange 키를 가진 요소가 사라졌음을 알 수 있기 때문에 페이지에서 해당 요소를 제거할 수 있고 apple이라는 key를 가진 요소가 그대로 남아있지만 string 요서 (예시에서는 과일 아이콘)만 변경됬음을 알고 해당 요소를 업데이트 할 수 있다.

참고 문서:
[Kent C. D blog](https://epicreact.dev/why-react-needs-a-key-prop/)
[리액트 공식문서](https://ko.reactjs.org/docs/reconciliation.html)
[Understanding the importance of the key prop in React - Franco D'Alessio](https://dev.to/francodalessio/understanding-the-importance-of-the-key-prop-in-react-3ag7)

```toc

```
