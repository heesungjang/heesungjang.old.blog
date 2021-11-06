---
emoji: 🔮
title: useState 지연 초기 state(lazy state initialization)
date: '2021-10-08 00:00:00'
author: 장희성
tags: react
categories: 프론트엔드
---

## Lazy initialization 🥱

## <지연 초기 state>

리액트에서 특정 함수나 실행 시간이 오래 걸리는 값 비싼 연산이 필요할때 useCallback이나 useMemo로 최적화를 한다.

이번 블로그에서는 useState을 사용해서 초기 상태(state) 값을 할당할때 값 비싼 연산이 필요한 함수를 `useState(expensiveFunction())`이런식으로 바로 파라미터로 전달하면 발생하는 **문제점과 해결 방법**을 정리해보자.

```javascript
// Greeting 컴포넌트
const GreetingComponent = (props) => {
  const getLocalStorageValue = () => {
    return window.localStorage.getItem('name') || initialName;
  };

  const [name, setName] = useState(getLocalStorageValue());

  React.useEffect(() => {
    window.localStorage.setItem('name', name);
  }, [name]);

  return (
    <div>
      <form>
        <label htmlFor="name">Name: </label>
        <input value={name} onChange={handleChange} id="name" />
      </form>
      {name ? <strong>Hello {name}</strong> : 'Please type your name'}
    </div>
  );
};
```

<hr/>

## 예시🕵️‍♀️

![](https://images.velog.io/images/heesungj7/post/4c8725ce-458d-4013-8592-71e21afc198e/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.35.51.png)

1. 사용자가 Input창에 이름을 입력하면 입력 값을 name 값에 업데이트하고 Hello {name}으로 사용자가 입력한 이름을 화면에 그려주는 간단한 컴포넌트이다.

2. 새로고침 이후에도 사용자가 마지막으로 입력한 값을 화면에 그려주기 위해서 useEffect의 의존 값을 name을 설정하고 name 값의 변화를 감지하면 localStorage에 저장하고

3. 컴포넌트가 리랜더링되면 useState에서 getLocalStorageValue() 함수를 통해서 로컬스토리지 값을 초기 name 값으로 설정한다.

## ❌ 어떤 문제가 있을까?

![](https://images.velog.io/images/heesungj7/post/7654a231-891b-46f4-b7b1-0a878ed5f2f7/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.45.22.png)

```javascript
const getLocalStorageValue = () => {
  console.log('i am an expensive function!!');
  return window.localStorage.getItem('name') || initialName;
};
```

이렇게 `getLocalStorageValue()` 함수안서 로그를 찍어보면 해당 함수가 인풋 창에 입력되는 네임 값이 변할때 마다 실행되고 있는걸 볼 수 있다.

![](https://images.velog.io/images/heesungj7/post/9333094b-8b00-4abf-a51e-2e62dbdc84b0/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.46.01.png)

우리는 초기에 컴포넌트가 실행될때 딱 한번만 로컬스토리지에서 값을 받아오면 되는데 리렌더링이 발생할때 마다 불필요한 함수가 실행되는 것이다.

```javascript
useState(() => getLocalStorageValue());
```

![](https://images.velog.io/images/heesungj7/post/d9117a2b-ce64-4314-8e35-33851d937097/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-10-08%20%E1%84%8B%E1%85%A9%E1%84%8C%E1%85%A5%E1%86%AB%2010.59.32.png)

이렇게 고비용 계산을 초기 initialState 값으로 전달해야 하는 경우에 함수 형태로 useState을 변경해주면 해당 함수는 컴포넌트가 첫 랜더링할때 한번만 실행되고 이후 리랜더링 상황에서는 다시 실행되지 않는다.

```toc

```
