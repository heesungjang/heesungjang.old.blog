---
emoji: ✏️
title: Next에서 데이터 호출하기
date: '2021-11-20 00:00:00'
author: 장희성
tags: react next swr
categories: 프론트엔드
---

클라이언트에서 외부 데이터가 필요할때 즉, 서버에 api 요청을 보내야할때 axio를 계속해서 사용 해 왔다.

그러다 얼마 전 회사에서 신규 프로젝트를 next로 개발하게 되었는데 graphql 서버와 통신하기 때문에 해당 프로젝트에서는 자연스럽게 apollo-client를 사용했지만 next 공부를 위해서 공식 문서를 둘러보다 SWR에 대해서 알게 되었다.

요즘 리액트 진영에서 핫한? 프레임워크 이기때문에 Next.js가 클라이언트 및 서버 측 렌더링을 모두 지원하는 것을 알고 있을 것이다. Next에서는 데이터를 가져오는 여러 가지 방법을 제공하는데 그중 하나가 next가 직접 개발한 SWR이다.

`공식 문에서는 SWR을 "데이터 가져오기를 위한 React Hooks"`이며 SWR을 사용하면 컴포넌트는 지속적이며 자동으로 데이터 업데이트 스트림을 받게 되고 그로인해 UI는 항상 빠르고 반응한다고 소개한다.

이번 글에서는 기본적인 SWR 개념 및 사용법과 SWR이 제공하는 caching, pagination, revalidation 등의 기능을 살펴보고자 한다.

<hr/>

## What is SWR?

SWR은 stale-when-revalidate의 약자이며 앞에서 언급했듯이Next.js와 같은 팀에서 만든 데이터 요청을 위한 경량 라이브러이다. SWR은 데이터베이스의 특정 상태(정확히는 API의 응답)를 직접 컴포넌트로 연결하기 때문에 개발자는 useSWR 을 통해 원격 서버의 특정 상태에 연결된 스트림을 얻을 수 있으며 그 스트림을 통해 원격 상태에 접근할 수 있고 이를 통해 데이터를 직접 화면에 뿌려줄 수 있다 (e.g 로딩중일때 spinner를 화면에 랜더링).

🤔 자, 그럼 swr은 데이터를 가져오는 데 axios에 비해 무엇이 좋은가?

### 포커싱하면 데이터가 갱신된다.

axios는 한 번 get으로 호출을 하면 다시 호출하지 않는 이상 이전의 데이터를 그대로 유지하는 반면 swr은 최초 한 번만 호출해도 다른 곳으로 포커싱을 옮겼다가 다시 포커싱하면 새로운 데이터로 갱신된다.

### 주기적인 호출도 가능하다.

이 기능은 apollo-client의 useQuery hooks에서도 제공하는 기능이고 실제 회사 프로젝트에서 아주 유용하게 사용했던 기억이 있다.

> 비대면 진료 프로젝트를 진행할때 환자-의사 매칭을 위해 의사분들이 사용하는 데스크탑 페이지를 따로 구축 했는데 소켓을 사용하지 않고 실시간으로 들어오는 환자들의 진료 신청을 대기중인 환자 리스트 페이지에 업데이트하고 싶었는데 이때 usequery가 제공하는 **Polling** 기능이 아주 유용했다.

비슷한 상황에서 SWR을 사용하고 있다면 Websocket 만큼은 아니지만 polling 방식을 이용해 Realtime을 구현 해보는 것을 적극 추천한다.

### 캐시된 데이터를 이용한다.

만약에 데이터를 가져오는 데 오래 걸리는 호출이 있다고 가정하자. 한 5초 정도? 그럼 해당 페이지를 들어갈 때마다 axios는 5초동안 데이터를 가져올 것이다.

하지만 swr은 최초에 데이터를 수집한 후에는 계속해서 오래 걸리는 호출 방식을 고수하는 것이 아니라 캐시된 데이터를 이용해서 효율적인 동작을 만들어낸다.

물론 캐시 이후에는 자동적으로 revalidate하여 데이터의 일관성을 유지해준다.

그 외에도 pagination 후에도 이전 스크롤 위치를 자동으로 기억해준다던지, 먼저 적용하고 이후 revalidation에서 규칙을 적용시켜준다던지 (local mutation: a good way to make changes feel faster) useSWR들 간에 의존성을 지원해준다던지 여러가지 추가 기능들을 제공해준다.

## 데이터 호출하기

### 1. Fetching the data with useSWR

SWR는 기본적으로 "useSWR" 과 "useSWRnfinite", 두개의 hooks를 제공하는데 useSWRnfinite은 페지네이션을 데이터에 매길 수 있게 해준다. 이를 통해서 infinity scroll(무한 스크롤)등을 쉽게 구현 할 수 있다.

✏️ 자, 이제 코드를 통해 살표보자:

-> useRequest.js

```javascript
import useSWR from 'swr';

const fetcher = (url) => fetch(url).then((res) => res.json());
const baseUrl = 'https://jsonplaceholder.typicode.com';

export const useGetPosts = (path) => {
  if (!path) {
    throw new Error('Path is required');
  }

  const url = baseUrl + path;

  const { data: posts, error } = useSWR(url, fetcher);

  return { posts, error };
};
```

위 예제에서는 useGetPosts라는 커스텀 훅스를 만들어서 사용했지만 데이터 요청이 이뤄지는 컴포넌트 안에서 직접 사용해도 무관하다.

fetcher 함수는 HTTP 요청을 서버로 보낸 다음 응답 데이터를 JSON으로 형식으로 파싱해준다. fetcher 메서드는 Next.js가 기본적으로 제공되기 때문에 별도의 import나 모듈 설치가 필요하지 않다.

-> components/Post.js

```javascript
export default function Post({ post }) {
  const { title, body, id } = post;
  return (
    <div className="Card">
      <h1 className="Card--title">
        {id}. {title}
      </h1>
      <p className="Card--body">{body}</p>
    </div>
  );
}
```

-> App.js

```javascript
import { useGetPosts } from '../useRequest';
import Post from '../components/Post';

export default function IndexPage() {
  const { posts, error } = useGetPosts('/posts');

  if (error) return <h1>Something went wrong!</h1>;
  if (!posts) return <h1>Loading...</h1>;

  return (
    <div className="container">
      <h1>My Posts</h1>
      {posts.map((post) => (
        <Post post={post} key={post.id} />
      ))}
    </div>
  );
}
```

위 예시에서 useSWR 을 통해 얻은 data 를 원격상태에 연결된 스트림으로 바라볼 수 있다. App 컴포넌트는 지금 직접 원격 서버의 상태를 화면에 뿌려주고 있는 것이다. 원격의 상태는 비동기적일 수 밖에 없기 때문에 useSWR은 데이터(data) 뿐만 아니라 에러(error)와 로딩상태(isValidating)를 함께 리턴한다.

리액트 컴포넌트는 3가지 리턴값을 이용해 서버의 상태를 실시간(에 가깝게)으로 화면에 표현할 수가 있다. 그리고 서버에서 post 데이터 값이 바뀌면 자동으로 화면에 새롭게 갱신된다. useState 와 useEffect 없이 이런 일이 가능하다는 것은 놀라운 점이다.

```toc

```
