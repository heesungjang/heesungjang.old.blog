---
emoji: ✏️
title: 리덕스 createAsyncThunk로 비동기 처리하기
date: '2021-07-12 00:00:00'
author: 장희성
tags: redux
categories: 프론트엔드
---

## 🤔 리덕스는 어떻게 관리할까?

리덕스는 어떻게 관리할까? 프로젝트를 시작하면서 고민을 많이했다.

이전 프로젝트를 할때는 redux의 action과 reducer를 직접 만들어주는 방식으로 vanilla redux로 작업을했다.

마지막 6주 협업을 진행하게 되면서 분명 프로젝트의 규모가 커질것이라고 생각했고 redux-createAsyncThunk를 사용해서 비동기 미들웨어와 리듀서들을 조금더 효율적이고 간결하게 관리하기로 했다.

### Create Async Thunk

- redux 에서 비동기 처리를 할경우 보통 thunk, saga, redux-observable 등의 미들웨어를 사용하여 한개의 비동기 액션에 대해 pending(비동기 호출 전), success(비동기 호출 성공), failure(비동기 호출 실패) 의 상태를 생성하여 처리하는 경우가 많다.

- 이때 각 상태를 만드는것은 각자 유틸 패키지를 받거나 직접 구현하여서 사용하였는데 이를 redux-toolkit 에서 createAsyncThunk 을 통해 구현이 가능하다.

```javascript
>> redux/async/user

export const signupUserDB = createAsyncThunk(
    "user/signup",
    async (data, thunkAPI) => {
        try {
            //회원가입 요청 api
            const response = await userApi.signup(data);
            if (response.data.ok) {
                // 회원가입 성공시 리듀서로 성공 여부 반환
                return true;
            }
        } catch (err) {
            // 에러 메세지 반환
            if (!err.response.data.ok) {
                // 이메일 중복 등  서버 pre set 에러 메세지 반환
                Sentry.captureException(`error, 회원가입 서버 : ${err}`);
                return thunkAPI.rejectWithValue(err.response.data.message);
            } else {
                Sentry.captureException(`error, 회원가입 통신 : ${err}`);
                // 서버 또는 api 통신중 발생하는 에러 메세지 반환
                Swal.fire(
                    "에러",
                    "회원가입에 실패하였습니다. 다시 시도해주세요!",
                    "error",
                );
                return thunkAPI.rejectWithValue(err.response.errorMessage);
            }
        }
    },
);

```

**createAyncThunk**를 사용해서 회원가입 비동기 액션을 만들어줬다. 회원가입 페이지에서 아래와같이 해당 thunk를 dispath() 함수를 통해서 호출해주면 첫번째 파라미터로 선언한 액션 이름 에 **pending, fulfilled, rejected 의 상태에 대한 action 을 자동으로 생성해준다.**

```javascript

dispatch(signupUserDB({유저정보}))

[signupUserDB.pending]: (state, action) => {
            state.isFetching = true;
        },
[signupUserDB.fulfilled]: (state, { payload }) => {
            state.isFetching = false;
            state.errorMessage = "";
            state.isSignupSuccess = payload;
        },
[signupUserDB.rejected]: (state, { payload: errorMessage }) => {
            state.errorMessage = errorMessage;
            state.isFetching = false;
        },
```

이런식으로 rejected되었을때 상태를 바로 접근할 수 있어서 서버에서 보내주는 에러 메세지나 예상치 못하게 발생하는 에러에 대한 예외처리를 할 수 있었다.

```toc

```
