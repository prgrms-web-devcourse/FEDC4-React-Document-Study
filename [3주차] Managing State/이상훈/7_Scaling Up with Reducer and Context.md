> 이번 파트에서 배울 내용
> - reducer와 context를 결합하는 방법  
> - 반복적인 prop 전달을 context를 통해 대체하는 방법  
> - state와 dispatch 함수를 prop으로 전달하지 않는 방법  
> - context와 state 로직을 별도의 파일에서 관리하는 방법  

## reducer와 context를 결합하기
상위 컴포넌트에서 `useReducer` 를 사용하고, 하위 컴포넌트에서 이 것에 접근하기 위해서는 `props` 로 전달해줘야 합니다. 그런데 컴포넌트 깊이가 깊어질수록 `props drilling` 현상이 심화되다보니 `context` 를 함께 사용해주는 방법이 있습니다.  

reducer와 context를 결합하는 방법은 크게 3단계입니다.  

### 1. Context 생성
```js
import { createContext } from 'react';

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);
```

### 2. state와 dispatch 함수를 context에 넣기
```jsx
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
  // ...
  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        <AddTask />
        <TaskList />
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

### 3. 하위 컴포넌트 안에서 context 사용
```jsx
export default function TaskList() {
  const tasks = useContext(TasksContext);
  // ...
}
```

## 하나의 파일로 합치기
reducer와 context를 모두 하나의 파일에 작성하고, 일종의 Provider 컴포넌트를 반환하거나 컨텍스트를 가져오는 훅을 반환하는 방식으로 코드를 정리하면 더욱 손쉽게 이용할 수 있습니다.

```jsx
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

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

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```
