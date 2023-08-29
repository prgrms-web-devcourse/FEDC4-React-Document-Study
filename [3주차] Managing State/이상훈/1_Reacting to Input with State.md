> 이번 파트에서 배울 내용
> - 선언적 UI 프로그래밍과 명령적 UI 프로그래밍의 차이점  
> - 컴포넌트에 들어갈 수 있는 다양한 시각적 state를 열거하는 방법  
> - 다른 시각적 상태의 변화를 트리거하는 방법  

## 명령형 프로그래밍
UI를 그리기 위해서 필요한 모든 인터랙션을 일일히 지정하는 방법입니다.  
시스템이 복잡해질수록 버그가 발생하기 쉽습니다.  

```js
async function handleFormSubmit(e) {
  e.preventDefault();
  disable(textarea);
  disable(button);
  show(loadingMessage);
  hide(errorMessage);
  try {
    await submitForm(textarea.value);
    show(successMessage);
    hide(form);
  } catch (err) {
    show(errorMessage);
    errorMessage.textContent = err.message;
  } finally {  
    hide(loadingMessage);
    enable(textarea);
    enable(button);
  }
}

function handleTextareaChange() {
  if (textarea.value.length === 0) {
    disable(button);
  } else {
    enable(button);
  }
}

function hide(el) {
  el.style.display = 'none';
}

function show(el) {
  el.style.display = '';
}

function enable(el) {
  el.disabled = false;
}

function disable(el) {
  el.disabled = true;
}

function submitForm(answer) {
  // 네트워크에 접속한다고 가정해봅시다.
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (answer.toLowerCase() == 'istanbul') {
        resolve();
      } else {
        reject(new Error('Good guess but a wrong answer. Try again!'));
      }
    }, 1500);
  });
}

let form = document.getElementById('form');
let textarea = document.getElementById('textarea');
let button = document.getElementById('button');
let loadingMessage = document.getElementById('loading');
let errorMessage = document.getElementById('error');
let successMessage = document.getElementById('success');
form.onsubmit = handleFormSubmit;
textarea.oninput = handleTextareaChange;
```

## 선언형 프로그래밍
UI에서 사용되는 상태, HTML 요소, 이벤트 핸들러 등을 선언하는 방식입니다.  
리액트에서 선언형 프로그래밍을 구현하기 위해서는 크게 5가지의 과정을 거칩니다.  

### 1. 컴포넌트의 다양한 시각적 state 확인하기
사용자가 볼 수 있는 UI의 모든 state 를 분석합니다.  

```jsx
export default function Form({
  status = 'empty'
}) {
  if (status === 'success') {
    return <h1>That's right!</h1>
  }
  return (
    <>
      <h2>City quiz</h2>
      <p>
        In which city is there a billboard that turns air into drinkable water?
      </p>
      <form>
        <textarea />
        <br />
        <button>
          Submit
        </button>
      </form>
    </>
  )
}
```

### 2. 무엇이 state 변화를 트리거하는지 알아내기
어떤 인터랙션이 state를 변화시키는지를 알아냅니다.  

### 3. 메모리의 state를 useState로 표현하기
`useState` 를 통해서 컴포넌트가 가져야 할 시각적 state를 선언적으로 표현합니다.  

```jsx
const [isEmpty, setIsEmpty] = useState(true);
const [isTyping, setIsTyping] = useState(false);
const [isSubmitting, setIsSubmitting] = useState(false);
const [isSuccess, setIsSuccess] = useState(false);
const [isError, setIsError] = useState(false);
```

### 4. 불필요한 state 변수를 제거하기
중복적이거나 불필요한 state는 제거합니다.  

- **state 간의 동기화가 잘 이뤄지지 않는 경우**: 예를 들면 `isTyping` 과 `isSubmitting` 이 동시에 true일 수는 없습니다. 이 경우 하나의 state 로 합칠 수 있습니다.  
- **다른 state 변수에 이미 같은 정보가 담겨진 경우**: isEmpty와 isTyping은 동시에 true가 될 수 없습니다. 이를 각각의 state 변수로 분리하면 싱크가 맞지 않거나 버그가 발생할 위험이 있습니다. 따라서 하나의 state로 합칠 수 있습니다.  
- **다른 state 변수를 활용해서 같은 정보를 얻을 수 있는 경우**: isError는 error !== null로도 대신 확인할 수 있기 때문에 필요하지 않습니다.  

```jsx
const [answer, setAnswer] = useState('');
const [error, setError] = useState(null);
const [status, setStatus] = useState('typing'); // 'typing', 'submitting', or 'success'
```

### 5. state 설정을 위해 이벤트 핸들러를 연결하기
원하는 인터랙션마다 이벤트 핸들러를 등록해서 state를 변화시킵니다.  
그러면 리액트는 상태 변화를 감지하여 화면을 다시 그리게 됩니다.  
