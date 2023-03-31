# Redux 구조 및 예시

⭐ redux에서 중요한 것은 상태를 직접 변경하는 것이 아닌 "**action**"을 통해 변경하는 것 ⭐

> state
>> store에서 저장되어 있는 값.
> action
>> store에 저장된 값을 변경시키는 방식을 정함.
> reducer
>> (action, old state)를 받아서 new state로 변환시키는 함수.
> store
>> redux를 이용하는 이유, store에 state를 저장.


## 예제

> 숫자 카운트 예제
> - reducer를 2개 사용. (rootReducer를 이용하기 위해)
> - add, sub, reset 할 수 있는 reducer (addsubReducer)
> - 버튼 누른 횟수만 체크하는 reducer (countiongReducer)
> - main화면에서 두 개의 state를 보여주는 방식으로 진행

### 1. action

action이란 type 필드를 가지는 js object로 현재 프로그램에서 무엇이 발생하는지 묘사하는 것

ex) ```{type: 'increment'} , {type: 'addList', payload: text}```

=> 전달할 data가 있다면 payload로 전달하는 것이 관행
