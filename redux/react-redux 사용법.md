# react-redux 사용법

## 사용하는 이유

### state 종속성 탈피

우리는 `useState`를 사용할 경우 컴포넌트 내부에 state를 만들고, 함수로 state를 바꿉니다.
그렇기 때문에 state는 컴포넌트에 종속되는 것은 당연한 결과 입니다.
**redux**는 컴포넌트에 종속되지 않고, 상태관리를 컴포넌트 바깥에서 합니다.
프로젝트 루트레벨에서 `store`라는 곳에 state를 저장하고, 모든 컴포넌트는 store에 `구독`을 하면서 `state`와 그 state를 `바꾸는 함수`를 전달 받게 된다.
함수를 바꿈으로 state가 바뀌면 해당 state를 바라보고 있는 컴포넌트는 모두 리렌더링 됩니다.

## redux 기본 원리
redux는 기본적으로 `flux 패턴`을 따른다.

```Action -> Dispatch -> Store -> View```

redux의 데이터 흐름은 동일하게 단방향으로 `view(컴포넌트)`에서 `Dispatch(store에서 주는 state를 바꾸는 함수)`라는 함수를 통해 `action(디스패치 함수 이름)`이 발동되고
`reducer`에 정의된 로직에 따라 `store의 state` 가 변화되고 그 `state`를 쓰는 `view(컴포넌트)`가 변하는 흐름을 따릅니다.

## Redux?

### Redux 등장 배경

MVC 패턴 형식으로 state가 변화되면 Model, View, Controller에 이벤트가 발생하고 값이 변화하는 구조이다.
즉, 양방향 데이터 흐름
양방향 데이터 흐름은 복잡하고 데이터 흐름을 판단하기 힘든 구조입니다.
이러한 해결방법으로 단방향 데이터 흐름이 있다. 그게 바로 **Redux**

Redux는 MVC 패턴에서의 단점을 개선하는 것이 목적이었고 그 해결책으로 단방향 데이터 흐름을 적용
View는 MVC 패천과 달리 데이터를 변경시키지 않고 Action을 넘겨줌
이때, Action은 반드시 Dispatcher를 통해서 데이터가 변경된다.
변경된 데이터를 Store를 통해 View가 전달받는다.

### Redux 장점

가장 많이 사용하는 리액트 상태 관리 라이브러리이다.
Redux를 사용하면 컴포넌트의 상태 업데이트 관련 로직을 다른 파일로 분리시켜서 더욱 효울적으로 관리할 수 있다.
또한, 컴포넌트끼리 똑같은 상태를 공유해야 할 때도 여러 컴포넌트를 거치지 않고 손쉽게 상태 값을 전달하거나 업데이트를 할 수 있다.

### react-redux

react-redux도 redux와 마찬가지로 데이터를 스토어 > 컴포넌트 > 액션 > 리듀서 > 스토어의 과정을 통해 변경
- 차이점은 스토어 > 컴포넌트, 컴포넌트 > 액션 단계에서 connect라는 react-redux 함수가 사용된다는 것

## Action
상태에 어떠한 변화가 필요하면 action 이란 것이 발생.
이는 하나의 객체로 표현되는데, 액션 객체는 다음과 같은 형식으로 이루어져 있다.
```javascript
{
    type: 'INCREMENT'
}
```

action 객체는 type 필드를 반드시 가지고 있어야 합니다.
이 값을 액션의 이름이라고 생각하면 된다. 그리고 그 외의 값들은 나중에 상태 업데이트를 할 때 참고해야 할 값이며, 작성자 마음대로 넣을 수 있다.
```javascript
{
    type: 'CHANGE',
    data: {
    	id: 1
        name: '코딩병원'
    }
}

{
    type: 'INPUT',
    text: '코딩병원119'
}
```

어떤 변화를 일으켜야 할 때마다 액션 객체를 만들어야 하는데 매번 액션 객체를 직접 작성하기 번거로울 수 있고, 만드는 과정에서 실수로 정보를 놓칠 수도 있다.
이러한 일을 방지하기 위해 이를 함수로 만들어서 관리한다.
```javascript
const input = text => ({
    type: 'INPUT',
    text
})
```

## Reducer

리듀서는 변화를 일으키는 함수이다.
액션을 만들어서 발생시키면 리듀서가 현재 상태와 전달받은 액션 객체를 파라미터로 받아 온다.
그리고 두 값을 참고하여 새로운 형태를 만들어서 반환.
리듀서에서는 상태의 불변성을 유지하면서 데이터에 변화를 일으켜 주어야 합니다.
이 작업을 할 때 spread 연산자(...) 를 사용하면 편하다.

```javascript
const initialState = {
	toggle: false,
    counter: 1
};

// state가 undefined이면 initialState를 기본값으로 사용
function reducer(state = initialState, action) {
    // action type에 따라 작업 처리
    switch(action.type) {
  	case TOGGLE:
        	return {
                // 불변성 유지
            	...state, 
                toggle: !state.toggle;
            }
    	case INCREMENT:
        	return {
            	...state,
            	counter: state.counter + 1
            }
        case DECREMENT:
        	return {
            	...state,
                counter: state.counter - 1
            }
        default:
        	return state;
    }
}
```

Reducer에서 action.type에 따라 reducer가 실행될 때, state의 값을 변경할 때는 Spread Operator(...) or Object.assign()으로 
기존 state를 복사하여 사용해야 하는 이유는 다음과 같다.
Redux는 reducer를 거친 state가 변경 되었는지를 검사하기 위해 state 객체의 주소를 비교한다.
state의 복제본을 만들어 반환하면 이전의 state와 다른 주소 값을 가리키기 때문에 state가 변경되었다고 판단한다.
반대로 state를 복제하는 것이 아닌 속성만 수정해서 반환하면 기존의 state와 가리키는 주소 값이 같기 때문에 변경 감지가 되지 않는다.

## Store

프로젝트에 리덕스를 적용하기 위해 스토어를 만든다.
한 개의 프로젝트는 단 하나의 스토어만 가질 수 있다.
스토어 안에는 현재 애플리케이션 상태와 리듀서가 들어가 있으며, 그 외에도 몇가지 중요한 내장 함수를 지닌다.

```javascript
import { createStore } from 'redux';

(...)

// 파라미터에는 리듀서 함수를 넣어주어야 함
const store = createStore(reducer);
```

## Dispatch

디스패치는 스토어의 내장 함수 중 하나이다.
디스패치는 '액션을 발생시키는 것'이라고 생각하면 된다.
이 함수는 dispatch(action)과 같은 형태로 액션 객체를 파라미터로 넣어서 호출한다.
이 함수가 호출되면 스토어는 리듀서 함수를 실행시켜서 새로운 상태를 만들어 준다.

```javascript
import { createStore } from 'redux';

(...)

// 파라미터에는 리듀서 함수를 넣어주어야 함
const store = createStore(reducer);

button.onclick = () => { 
   // dispatch 함수를 사용하여 액션을 스토어에게 전달
   store.dispatch(plus(1));
}
```

## Redux의 3가지 규칙
1. 단일 스토어
> 하나의 애플리케이션 안에는 하나의 스토어가 들어 있다.
> 사실 여러개의 스토어를 사용하는 것이 불가능 하지는 않지만 상태 관리가 복잡해 질 수 있어 권장하지 않는다.

2. 읽기 전용 상태
> 리덕스 상태는 읽기 전용이다.
> 상태를 업데이트할 때 기존의 객체를 건드리지 않고 새로운 객체를 생성해 주어야 한다.
> 리덕스에서 불변성을 유지해야하는 이유는 내부적으로 데이터가 변경되는 것을 가지하기 위해 얕은 비교 검사를 한다.
> 객체의 변화를 감지할 때 객체의 깊숙한 안쪽까지 비교한느 것이 아니라 겉핥기 식으로 비교하여 좋은 성능을 유지할 수 있는 것입니다.

3. Reducer는 순수한 함수
> 변화를 일으키는 리듀서 함수는 순수한 함수여야 한다. 순수한 함수는 다음 조건을 만족한다
> - 리듀서 함수는 이전 상태와 액션 객체를 파라미터로 받는다.
> - 파라미터 외의 값에는 의존하면 안된다.
> - 이전 상태는 절대로 건드리지 않고, 변화를 준 새로운 상태 객체를 만들어서 반환힌다.
> - 똑같은 파라미터로 호출된 리듀서 함수는 언제나 똑같은 결과 값을 반환해야 한다.

리듀서를 작성할 때는 위 네가지 상황을 주의해야한다.
예를들어 리듀서 함수 내부에서 랜덤 값을 만들거나, Date 함수를 사용하여 현재 시간을 가져오거나, 네트워크 요청을 한다면, 파라미터가 같아도 다른 결과를 마들어 낼 수 있기 때문에
사용하면 안된다. 이런 작업은 리듀서 함수 바깥에서 처리해 주어야 한다.
액션을 만드는 과정에서 처리해도 되고, 리덕스 미들웨어에서 처리해도 됩니다. 주로 네트워크 요청과 같은 비동기 작업은 미들웨어를 통해 관리한다.
























