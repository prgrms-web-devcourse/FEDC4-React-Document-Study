
#  state 로직을 reducer를 이용하여 구성하기

컴포넌트가 복잡해지면 컴포넌트의 state가 업데이트 되는 다양한 경우를 파악하기 어려워질 수 있다. reducer를 사용한다. 이벤트 헨들러에서 action을 전달하여, 사용자가 '한 일'을 구체적으로 정한다. (예를 들어, 태스크 추가,변경,삭제)


dispatch 함수안의 객체를 action이라 한다.
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
    task: task,
  });
}

function handleDeleteTask(taskId) {
  dispatch({
    type: 'deleted',
    id: taskId,
  });
}
```

reducer 함수의 간단한 예시이다.
```jsx
function yourReducer(state, action) {
  // return next state for React to set
}
```
state는 현재 state를 나타내고, action은 action객체를 나타낸다. 해당 함수는 다음 state를 반환한다.
다음의 예시 코드가 있다.

```jsx
App.js
import { useReducer } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';
import tasksReducer from './tasksReducer.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

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
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: 'deleted',
      id: taskId,
    });
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}

let nextId = 3;
const initialTasks = [
  {id: 0, text: 'Visit Kafka Museum', done: true},
  {id: 1, text: 'Watch a puppet show', done: false},
  {id: 2, text: 'Lennon Wall pic', done: false},
];
tasksReducer.js
export default function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [
        ...tasks,
        {
          id: action.id,
          text: action.text,
          done: false,
        },
      ];
    }
    case 'changed': {
      return tasks.map((t) => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}
```

### useReducer
useReducer를 사용하면 reducer 함수와 action을 전달하는 부분을 작성한다.
업데이트 로직이 어떻게 동작하는지, 이벤트 헨들러를 통해 무엇이 일어났는지 쉽게 알 수 있다.
reducer는 순수해야 한다. (같은 입력, 같은 출력)