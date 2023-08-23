## 이벤트 응답
***
이벤트 핸들러 추가
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
***
인라인으로 이벤트 헨들러 정의도 가능하다.
```jsx
<button onClick={function handleClick() {
  alert('You clicked me!');
}}>
```
***
화살표 함수도 가능하다.
```jsx
<button onClick={() => {
  alert('You clicked me!');
}}>
```
***
이벤트 헨들러에 함수를 전달할 때에는, 호출이 아니라 전달되도록 주의해야 한다.
```jsx
<button onClick={handleClick}> o

<button onClick={handleClick()}> x
    ```
***
이벤트 헨들러는 컴포넌트 내부에 선언되므로 prop값에 접근할 수 있다.
```jsx
function AlertButton({ message, children }) {
  return (
    <button onClick={() => alert(message)}>
      {children}
    </button>
  );
}

export default function Toolbar() {
  return (
    <div>
      <AlertButton message="Playing!">
        Play Movie
      </AlertButton>
      <AlertButton message="Uploading!">
        Upload Image
      </AlertButton>
    </div>
  );
}
```
***
이벤트 헨들러를 props로 전달
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
***
이벤트 헨들러 props 이름
```jsx
function Button({ onSmash, children }) {
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
***
이벤트 핸들러에 적절한 HTML 태그를 사용할 것. 예를 들어, <div>가 아닌 <button>으로 onClick 이벤트 헨들을 하는게 좋다.(키보드 탐색 가능) 
***
이벤트 전파

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
단, JSX에서 사용되는 onScroll 이벤트는 전파되지 않는다.

***
이벤트 전파 중지

이벤트 헨들러는 이벤트 객체를 유일한 인수로 받는다. 
해당 객체를 이용해서, e.stopPropagation()을 호출하여 이벤트 전파를 막을 수 있다.
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
  ```
  이벤트 전달이 중간에 막힌 경우에도 하위 요소의 모든 이벤트를 포착해야 하는 경우, (분석도구에 기록하고자 할 경우) 이벤트 이름 끝에 Capture을 붙인다. 
  ```jsx
  <div onClickCapture={() => { /* this runs first | 먼저 실행됨 */ }}>
  <button onClick={e => e.stopPropagation()} />
  <button onClick={e => e.stopPropagation()} />
</div>
```

## 기본 동작 방지
그 유명한 form의 submit 이벤트는 내부 버튼을 클릭할 때, 기본적으로 전체 페이지를 로드한다.
```jsx
export default function Signup() {
  return (
    <form onSubmit={() => alert('Submitting!')}>
      <input />
      <button>Send</button>
    </form>
  );
}
```
기본동작을 막기 위해,
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
***
렌더링 함수와 달리 이벤트 핸들러는 순수할 필요가 없다. 정보를 변경하기에는 이벤트 헨들러만한게 없다. 그렇다면, 정보를 변경하려면 그 전에 먼저 정보를 저장할 필요가 있다.