---
emoji: ✏️
title: Graphql은 REST API의 어떤점을 해결해주나?
date: '2021-09-17 00:00:00'
author: 장희성
tags: react
categories: 프론트엔드
---

> [1. GraphQL이란?
> ](https://velog.io/@heesungj7/Node-ApolloServer-GraphQL-API-%EC%84%9C%EB%B2%84-%EB%A7%8C%EB%93%A4%EA%B8%B01)에서 GraphQL(이하 gql)의 개념을 정리하면서 gql은 클라이언트 시스템에서 쿼리문을 작성한다고 짚고 넘어갔다.

## 🤔 클라이언트에서 쿼리를 작성하면 실제 어떤 장점이 있을까?

프로젝트를 진행하다보면 서버 개발자와 소통은 필수이다. 진행중인 서비스를 위해서 어떤한 데이터 또는 리소스를 사용자에게 보여주려면 일단 서버로부터 데이터를 받아와야한다. 이제는 웹 서비스를 "웹 어플리케이션"이라고 부를만큼 많은 액션들이 동적으로 일어나는데 당연히 데이터의 변화도 많고 그만큼 서버-클라이언트의 데이터 통신도 많다.

```javascript
/free/post
/free/post/:post_id
/free/post/:post_id/view_count
/free/post/:post_id (delete)
/free/post/:post_id (put)
/free/post/:post_id/like
```

백엔드 개발자는 보통 위 예시와 같은 각각의 endpoint를 만들고 프론트엔드 개발자가 이를 통해 데이터에 접근할수 있도록 공유 해야한다.

![](https://images.velog.io/images/heesungj7/post/25bdccf7-e3eb-4223-904b-e9200d8bb9ce/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202021-09-17%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%205.19.59.png)

기존 서버-클라이언트 협업 방식에서는 연동규격서라고 하는 API 명세서를 주고 받는 절차가 반드시 필요했다. 요즘은 swagger와 같이 API 문서화 자동화 툴도 많이 사용한다. 크고 작은 프로젝트를 진행하면서 이 API 명세서의 관리가 제대로 되지 않거나 인터페이스 변경 사항을 제때 문서에 반영하지 않아 프론트와 백엔드 개발자의 소통이 원할하게 이뤄지지 않는 경우를 많이 경험했다.

### 인트로스펙션(introspection)

gql은 서버 자체에서 현재 서버에 정의된 스키마의 실시간 정보를 공유를 할 수 있게하여 REST의 API 명세서 공유와 같은 문제를 해결해준다.

![](<https://images.velog.io/images/heesungj7/post/32bfd196-ccf9-4f1a-b343-d282f281c757/graphql-apollo-ide%20(2).png>)

서버에서 전체적인 **데이터의 모델 (스키마)를 정의해주면** 데이터의 tree 구조를 설명해주는 document가 자동으로 만들어주기 때문에 클라이언트를 작업하는 개발자는 자동적으로 만들어지는 인터랙티브한 document을 활용해서 필요한 쿼리를 만들어 사용할 수 있다.

```toc

```
