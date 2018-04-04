---
title: 왜 나는 Vue 를 선택 했는가?
date: 2018-04-04 23:36:52
tags: Vue
---
처음 React 를 배우고 실무에 적용 하면서 저는 몇가지 이유에 의해 Vue 전환 하기로 마음 먹었습니다.
현제 2018 년 기준으로 react 는 많은 보완 라이브러리가 개발되어 Vue 의 장점들 중 많은 부분을 
가지게 되었습니다. 그래도 모자르지요. 저가 Vue 를 선택한 이유는요 아래 장점 때문 입니다.

1. 실행 속도
2. 탬플릿
3. 완벽한 서버렌더링 지원
4. 낮은 학습곡선
5. 간결함
6. React 에 있는 모든 기능을 다 지원함
7. 플러그인

이 중 결정적 전환 결정을 한 계기는 플러그인 시스템입니다. 이 것은 node 서버 프레임워크를 express 에서
hapi 로 바꾼 이유와 같은데요 이게 뭐가 달라 다른 것도 비슷한 거 있어 라고 할 수도 있지만.

플러그인은 모든 추가 기능을 같은 규칙으로 한번만 정의 하면 되는 엄청나게 중요한 이점을 제공합니다.

그럼 우선 상태 관리 확장 라이브러리인 React 의 Redux 와 Vue 의 Vuex 를 작성 해 볼까요?

### Redux
```javascript
// reducers.js
export const INCREMENT = 'INCREMENT';
export const DECREMENT = 'DECREMENT';

export const increment = () => ({
  type: INCREMENT
});

export const decrement = () => ({
  type: DECREMENT
});

const initialState = {
  number: 0
}

export default (state = initialState, action) => {
  switch(action.type){
  case INCREMENT:
    return {
      ...state,
      number: state.number + 1,
    }
  case DECREMENT:
    return {
      ...state,
      number: state.number - 1,
    }
  default:
    return state
  }
}
```
```javascript
// index.js
import React from 'react'
import ReactDOM from 'react-dom'
import App from './containers/App'

import { createStore } from 'redux'
import reducers from './reducers'
import { Provider } from 'react-redux'

// 스토어 생성
const store = createStore(reducers);

ReactDOM.render(
    <Provider store={store}>
        <App />
    </Provider>,
    document.getElementById('root')
);
```

### Vuex
```javascript
// index.js
import Vue from 'vue'
import Vuex from 'vuex'
import App from './components/App'

// 스토어 생성
const store = new Vuex.Store({
  state: {
    number: 0
  },
  mutations: {
    increase(state) {
      state.number += 1
    },
    decrease(state) {
      state.number -= 1
    }
  }
})

Vue.use(Vuex)

new Vue({
  el: '#app',
  store,
  template: '<App/>',
  components: {App},
})
```

확장 라이브러리 (플러그인) 사용성과 간결함 규칙이 정해진 제작 방식은 Vue 최고의 강점이라고 저는 봅니다.

간결함은 또한 Jsx 를 쓰고 안쓰고에 많은 차이가 있습니다.
리엑트에도 Vue 와 비슷한 template 방식의 확장 라이브러리가 나왔습니다.
물론 Vue 도 Jsx 를 사용 할 수 있습니다.
다만 저가 Vue 로 옴길때엔 React template 라이브러리 기능이 만족 스럽지 못했고 Redux 와 같이 너무 많이 의미 없는
코드들을 써야만 했습니다.
특히 js 코드에 jsx 코드를 주입 하는 것은 마치 과거 시대의 PHP 를 보는 것 같아 너무 심리적 부담이 컷습니다.
전 심안을 가지고 있지 않아 다른 언어 안에 다른 언어가 들어가면 읽지를 못 합니다 ...
물론 React 를 그렇게 분리 해서 작성하면 되지만 다들 그렇게 안 작성 하시더라구요 ... ~~제발 분리 쩜 ...~~

그럼 코드 비교를 해볼까요 ?
우선 컴포넌트를 만들어 보겠습니다.
### React

```javascript
import React from 'react'

const HelloWorld = React.createClass({
  render: function() {
    return (
      <h1>Hello {this.props.name}!</h1>
    )
  }
})

export default HelloWorld
```
React 를 저가 처음 배울때 너무 당황스러 웠는데요. React 를 가져 왔는데 왜 아무대서도 안쓰는 걸까?
어라 왜 html 이 js 파일 안에 있지 그 것도 '' 도 없이... ~~아 php ...~~
~~그나저나 최신 예제에 props 설정은 어디 팔려간거져 React 님?~~
그럼 Vue 에서는 이런 혼돈 스러움을 없에 면서 기능 같은 기능 구현 하기 위해 어떻게 했을까요?

### Vue
```javascript
<template>
  <h1>Hello {{name}}!</h1>
</template>

<script>
  export default {
    props: {
      name: ''
    }
  }
</script>

```
Vue 는 Html 와 script 를 Vue 스타일로 분리 시킴니다. ~~너무 보기 좋지 않나요 ㅠㅠ 혼자 감격~~
그리고 두 html 과 script 는 코드 변동없이(중요) 파일로도 분리가 됩니다. 만들다가 길어지면 그냥 분리 하면 되지요

속도는 Vue 가 ~~상당히~~ 빠름니다. 아직도 브라우저는 느려서 속도가 참 중요합니다.
물론 리엑트도 열심히 여러 조건 맞춰서 프로그래머가 최적화 시키면 비슷한 속도가 나오지만
그럴 수록 코드 양은 늘어 나게 되고 읽기는 불편 해집니다. 다시말해 대부분 if 문을 vue 가 대신 해결해 주는 것 입니다.
~~자세한건 github 가서 코드를 까보세요~~

글 쓰다가 잠이와서... 이민 쓸께요 아무튼 처음에 리스트에 적은 ~~행운의~~ 7가지  이유로 저는 Vue 를 선택 했습니다.

Vue 는 React 와 Angular 의 장점을 잘 썩으면서 단점은 배제하고 거기에 Vue 만의 기능을 넣은 멋진 녀석 입니다.~~타도 React! Angular!~~

Vue 에 관심을 가져 보아요~

그럼 ~20000~ 안녕.