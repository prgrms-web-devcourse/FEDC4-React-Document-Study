# state 로직을 reducer로 추출하기

> 여러개의 state 업데이트가 여러 이벤트 핸들러에 분산되어 있는 컴포넌트인 경우 reducer를 통해 state 업데이트 로직을 통합할 수 있다.

## reducer로 state 로직 통합하기

> 이벤트 핸들러는 state를 업데이트 하기 위해 setState 함수를 호출한다. 복잡성을 줄이고 모든 로직을 접근하기 쉽게 한 곳에 모으려면, state 로직을 컴포넌트 외부의 reducer라고 하는 단일 함수로 옮긴다.

1. state를 설정하는 것에서 action들을 전달하는 것으로 변경하기

   - state를 설정하여 React에게 `무엇을 할지`를 지시하는 대신, 이벤트 핸들러에서 `action`을 전달하여 "사용자가 방금 한 일"을 지정한다.

   이벤트 핸들러로 state 업데이트

   ```jsx
   function handleAddTask(text) {
     setTasks([
       ...tasks,
       {
         id: nextId++,
         text: text,
         done: false,
       },
     ]);
   }
   ```

   reducer 함수의 dispatch 함수에 action 전달

   ```jsx
   function handleAddTask(text) {
     dispatch({
       type: "added",
       id: nextId++,
       text: text,
     });
   }
   ```

   <br>

2. reducer 함수 작성하기
   - reducer 함수는 두 개의 매개변수를 가지는데, 하나는 현재 state이고, 하나는 action 객체이다.
   ```js
   function yourReducer(state, action) {
     //return next state for React to set
   }
   ```
   - React는 reducer로부터 반환된 것을 state로 설정한다.(setState 로직)
   - reducer 함수는 state를 매개변수로 갖기 때문에, 컴포넌트 밖에서 reducer함수를 선언할 수 있다.
   - reducer함수 안에서 if/else 구문 보다는 switch구문을 사용하는게 일반적이다.

<br>

3. 컴포넌트에서 reducer 사용하기

   - 컴포넌트에 reducer 함수를 연결해야 한다. <br>

   **useReducer Hook import 하기**

   ```js
   import { useReducer } from "react";
   ```

   `useState` 대신 `useReducer`로 바꾼다.

   ```js
   const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
   ```

   `useReducer` Hook은 2개의 인자를 받는다

   1. reducer 함수
   2. 초기 state 함수

   <br>

   `useReducer` 다음을 반환한다.

   1. state 값
   2. dispatch 함수(사용자의 action을 reducer 에게 전달 해주는 함수)

## useState 와 useReducer 비교하기

1. Code size: 코드 크기 <br>
   `useState`: 미리 작성해야 하는 코드가 줄어든다.
   `useReducer`: 많은 이벤트 핸들러가 비슷한 방식으로 state를 업데이트 하는 경우 useReducer를 사용하면 코드를 줄이는데 도움이 된다.
2. Readability: 가독성 <br>
   `useState`: 간단한 state 업데이트는 가독성이 좋다.
   `useReducer`: 업데이트 로직이 어떻게 동작하는지와 이벤트 핸들러를 통해 무엇이 일어났는지를 깔끔하게 분리할 수 있다.
3. Debugging: 디버깅 <br>
   `useState`: 버그가 있는 경우, state가 어디가 잘못 설정되어 있는지, 왜 그런지 알기 어렵다.
   `useReducer`: reducer에 콘솔로그를 추가하여 state 업데이트와 왜 버그가 발생했는지 확인할 수 있다.
4. Testing: 테스팅<br>
   `useReducer`: 컴포넌트에 의존하지 않는 순수함수! 별도로 내보내거나 테스트 할 수 있다.
5. Personal preference: 개인 취향
