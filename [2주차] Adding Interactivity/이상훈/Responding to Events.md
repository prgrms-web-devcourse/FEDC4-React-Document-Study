> 이번 파트에서 배울 내용
> - 이벤트 핸들러를 등록하는 다양한 방법  
> - 상위 컴포넌트에 이벤트 처리 로직을 전달하는 방법  
> - 이벤트가 전파되는 방식과, 전파를 막는 방법  

## 이벤트 핸들러 등록
컴포넌트에 '클릭' 과 같은 이벤트에 대한 동작을 처리하는 핸들러를 등록하기 위한 과정은 다음과 같습니다.  

1. 컴포넌트 내부에 `handleClick` 과 같은 함수를 정의합니다.  
2. 함수 내부에서 수행해야 할 로직을 구현합니다.  
3. JSX로 작성하는 요소에 props로 이벤트 핸들러를 전달하여 등록합니다.  

```jsx
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

### 인라인 이벤트 핸들러
이벤트 핸들러 함수를 JSX에 인라인으로 바로 작성하는 방법도 있습니다.  

```jsx
<button onClick={function handleClick() {
  alert('You clicked me!');
}}>
```

```jsx
<button onClick={() => {
  alert('You clicked me!');
}}>
```

이벤트 핸들러 함수를 컴포넌트 내에 정의하는 방법과, 인라인으로 정의하는 방법은 동일한 결과를 낳습니다.  
그런데 인라인은 주로 간단한 처리를 할 때 편리합니다.  

### 주의사항
props로 전달하는 이벤트 핸들러는 함수 형태여야 하고, 호출해서는 안됩니다!!
호출하는 방식으로 작성한다면, 이벤트가 발생할 때 해당 핸들러가 호출되는게 아니라 렌더링될 때 해당 핸들러가 바로 호출되어 버립니다.  

```jsx
<button onClick={handleClick}> {/* 옳은 방법 */}
<button onClick={handleClick()}> {/* 잘못된 방법 */}
<button onClick={() => alert('You clicked me!')}> {/* 옳은 방법 */}
<button onClick={alert('You clicked me!')}> {/* 잘못된 방법 */}
```

## 이벤트 핸들러를 props로 전달
다른 컴포넌트에 이벤트 핸들러를 전달하는 방법도 가능합니다.  

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}

function PlayButton({ movieName }) {
  function handlePlayClick() {
    alert(`Playing ${movieName}!`);
  }

  return (
    <Button onClick={handlePlayClick}>
      Play "{movieName}"
    </Button>
  );
}

function UploadButton() {
  return (
    <Button onClick={() => alert('Uploading!')}>
      Upload Image
    </Button>
  );
}

export default function Toolbar() {
  return (
    <div>
      <PlayButton movieName="Kiki's Delivery Service" />
      <UploadButton />
    </div>
  );
}
```

## 이벤트 핸들러에 대한 이름 컨벤션
보통 이벤트 핸들러의 이름의 접두사로는 `handler` 을 붙히고, 다른 컴포넌트로부터 전달받은 이벤트 핸들러의 이름의 접두사로는 `on` 을 붙힙니다.  

```jsx
function Button({ onSmash, children }) {
  const handleClick = () => {
    console.log("click에 대한 처리!");
    onSmash && onSmash();
  }

  return (
    <button onClick={onSmash}>
      {children}
    </button>
  );
}

export default function App() {
  return (
    <div>
      <Button onSmash={() => alert('Playing!')}>
        Play Movie
      </Button>
      <Button onSmash={() => alert('Uploading!')}>
        Upload Image
      </Button>
    </div>
  );
}
```

## 이벤트 전파
하위 컴포넌트에서 발생한 이벤트는 그대로 상위 컴포넌트에도 전달이 됩니다.  

```jsx
export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <button onClick={() => alert('Playing!')}>
        Play Movie
      </button>
      <button onClick={() => alert('Uploading!')}>
        Upload Image
      </button>
    </div>
  );
}
```

예를 들어 위 예시 코드에서는 Play Movie 버튼을 누르면 다음과 같은 순서로 `alert` 창이 등장합니다.  

```
Play Movie
You clicked on the toolbar!
```

## 이벤트 전파 방지
만약 하위 컴포넌트에서의 이벤트가 상위 컴포넌트로 전파되는 현상을 방지하고 싶다면, 이벤트 객체의 `stopPropagation()` 메소드를 사용하면 됩니다.  

```jsx
function Button({ onClick, children }) {
  return (
    <button onClick={e => {
      e.stopPropagation();
      onClick();
    }}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div className="Toolbar" onClick={() => {
      alert('You clicked on the toolbar!');
    }}>
      <Button onClick={() => alert('Playing!')}>
        Play Movie
      </Button>
      <Button onClick={() => alert('Uploading!')}>
        Upload Image
      </Button>
    </div>
  );
}
```

## 기본 이벤트 방지
form의 `submit` 이벤트와 같이 브라우저마다 기본적으로 동작하는 이벤트를 막아야 하는 경우가 존재하는데요, 이벤트 객체의 `preventDefault()` 메소드를 호출하면 막을 수 있습니다.

```jsx
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```
