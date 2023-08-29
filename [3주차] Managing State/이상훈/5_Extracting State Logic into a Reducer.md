> 이번 파트에서 배울 내용
> - 리듀서 함수가 무엇인지  
> - `useState` 를 `useReducer` 로 리팩토링 하는 방법  
> - 언제 리듀서를 사용해야 하는지  
> - 리듀서를 잘 작성하는 방법  

## 리듀서란?  
상태를 업데이트하는 모든 로직을 담고 있는 하나의 단일 함수를 리듀서(reducer) 라고 합니다.  

보통 컴포넌트 내부에 선언되어 있는 상태나 이벤트 핸들러의 규모가 커지고 복잡해질수록 컴포넌트가 부담스러워질 수 있는데 이런 경우에 상태에 관련한 로직을 컴포넌트의 외부에 따로 빼서 사용할 때 유용합니다.  

### 왜 리듀서라고 불리는가?  
자바스크립트 배열의 `reduce()` 함수는 배열의 여러 값을 단일 값으로 누적하는 연산을 수행합니다.  

이 함수는 콜백 함수를 인자로 전달받게 되는데 보통 그 콜백 함수의 이름을 `reducer` 라고 하는 것에서 이름이 유래되었습니다.  

`reducer()` 함수의 인자인 콜백 함수가 `(지금까지의 결과, 현재 아이템)` 을 인자로 받듯이  
리액트의 리듀서도 `(지금까지의 상태, 현재 액션)` 을 인자로 받습니다.  

### 액션 객체란?
평범한 자바스크립트 객체이기에 어떤 모양이든 될 수 있습니다. 그러나 일반적으로 어떤 상황이 발생한 것인지를 표현하기 위해서 문자열 타입의 `type` 데이터를 넘겨주고, 이외의 추가 정보를 다른 필드에 담아두도록 작성하는 방식이 일반적입니다.  

```jsx
dispatch({
  type: 'what_happened',
  // ... 다른 필드
});
```

## 상태 로직을 리듀서로 통합하기
`useState` 와 컴포넌트 내부의 이벤트 핸들러로 구성되어 있는 로직을 `useReducer` 로 변경하는 과정은 크게 3단계입니다.  

### 1. 상태를 직접 수정하던 부분을 dispatch를 호출하는 방식으로 변경하기

#### before
```jsx
function handleAddTask(text) {
  setTasks([...tasks, {
    id: nextId++,
    text: text,
    done: false
  }]);
}

function handleChangeTask(task) {
  setTasks(tasks.map(t => {
    if (t.id === task.id) {
      return task;
    } else {
      return t;
    }
  }));
}

function handleDeleteTask(taskId) {
  setTasks(
    tasks.filter(t => t.id !== taskId)
  );
}
```

#### after
```jsx
function handleAddTask(text) {
  dispatch({
    type: 'added',
    id: nextId++,
    text: text,
  });
}

function handleChangeTask(task) {
  dispatch({
    type: 'changed',
    task: task
  });
}

function handleDeleteTask(taskId) {
  dispatch({
    type: 'deleted',
    id: taskId
  });
}
```

### 2. reducer 함수 작성하기
`(상태, 현재 수행할 액션)` 을 인자로 전달받는 순수 함수 형태의 리듀서 함수를 작성합니다.  

```jsx
function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```

### 3. 컴포넌트에서 reducer 사용하기
`useReducer()` 훅으로 리듀서를 사용합니다.  

```jsx
let nextId = 3;

const initialTasks = [
  { id: 0, text: 'Visit Kafka Museum', done: true },
  { id: 1, text: 'Watch a puppet show', done: false },
  { id: 2, text: 'Lennon Wall pic', done: false }
];

const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
```

## 리듀서 잘 작성하기
- **리듀서는 순수 함수여야 한다.**  
상태 변경 함수와 동일하게 리듀서는 순수 함수여야 합니다. 즉, 동일한 입력이 들어오면 항상 동일한 출력을 반환해야 하며 리듀서 내부에서 비동기 처리를 하거나 사이드 이펙트를 유발하는 로직을 작성해서는 안됩니다.  
- **각각의 액션은 여러 데이터를 변화시키더라도 단일 사용자 상호 작용을 표현해야 한다.**  
예를 들어서 사용자가 `초기화` 버튼을 눌렀을 때 변경해야 하는 데이터가 5개라고 할 경우, `reset_username`, `reset_password`, ... 등 액션을 여러개 만드는 것이 아니라 하나의 단일 동작을 표현하는 `reset` 으로 정의해야 합니다.  

## useState vs useReducer
- **코드 크기**: 일반적으로 `useState` 가 미리 작성해야 하는 코드가 적기 때문에 단순한 로직같은 경우에는 유리합니다. 그러나 로직이 복잡해질수록 `useReducer` 를 통해서 컴포넌트 외부에 정의된 리듀서를 가져와서 사용하는 것이 코드의 양을 줄이는데 도움이 됩니다.  
- **가독성**: 간단한 로직의 경우에는 `useState` 가 좋지만, 복잡한 구조의 로직일수록 `useReducer` 가 업데이트 로직과 이벤트 핸들러를 한 눈에 살펴보기 좋습니다.  
- **디버깅**: `useReducer` 를 사용하면 버그가 발생했을 때 왜 발생한 것인지 파악하기가 수월하지만, `useState` 를 사용했을 때 보다 더 많은 코드를 단계별로 실행해야 하는 점은 있습니다.  
- **테스팅**: 리듀서는 컴포넌트에 의존하지 않는 순수한 함수이기 때문에 독립적으로 테스트하기에 용이합니다.  
- **개인적인 취향**: `useState`, `useReducer` 를 각각 선호하는 사람의 취향에 따라 다를 수 있습니다.  
